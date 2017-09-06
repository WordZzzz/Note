**<font color="black" size=6 face="仿宋">NVIDIA Jetson TX1 系列开发教程之十一：TCP/IP文件传输</font>**

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **嵌入式平台：NVIDIA Jetson TX1**
- **嵌入式系统：Ubuntu16.04**
- **虚拟机系统：Ubuntu14.04**
- **编者: WordZzzz**

----------

**<font color="black" size=5 face="仿宋">写在前面：</font>**
本篇博文设计的内容是TCP/IP文件传输，只是简单的实现文档、图片等二进制文件的传输，并将大包进行分包发送，不涉及select等机制。

**<font color="black" size=5 face="仿宋">一、服务端程序：</font>**

```
/*
 * common.h
 *
 *  Created on: Aug 11, 2017
 *      Author: wordzzzz
 */

#ifndef COMMON_H_
#define COMMON_H_

#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <sys/socket.h>
#include <netinet/in.h>

#define PORT 6000
#define LISTENQ 20
#define BUFFSIZE 4096
#define FILE_NAME_MAX_SIZE 512

#endif /* COMMON_H_ */
```

```
/*
 * fileserver.c
 *
 *  Created on: Aug 11, 2017
 *      Author: wordzzzz
 */

#include "common.h"

int main(int argc, char **argv[])
{
    //Create socket
    int sockfd,connfd;
    struct sockaddr_in svraddr,clientaddr;
    bzero(&svraddr,sizeof(svraddr));
    bzero(&clientaddr,sizeof(clientaddr));

    svraddr.sin_family=AF_INET;
    svraddr.sin_addr.s_addr=htonl(INADDR_ANY);
    svraddr.sin_port=htons(PORT);

    sockfd=socket(AF_INET,SOCK_STREAM,0);
    if(sockfd<0)
    {
        perror("socket");
        exit(1);
    }

    //bind
    if(bind(sockfd,(struct sockaddr*)&svraddr,sizeof(svraddr))<0)
    {
        perror("bind");
        exit(1);
    }

    //listen
    if(listen(sockfd,LISTENQ)<0)
    {
        perror("listen");
        exit(1);
    }

    while(1)
    {
        socklen_t clientaddrlen=sizeof(clientaddr);

        //accept
        connfd=accept(sockfd,(struct sockaddr*)&clientaddr,&clientaddrlen);
        if(connfd<0)
        {
            perror("connect");
            exit(1);
        }

        //recv file imformation
		char buff[BUFFSIZE];
		char filename[FILE_NAME_MAX_SIZE];
		int count;
		bzero(buff,BUFFSIZE);

		count=recv(connfd,buff,BUFFSIZE,0);
		if(count<0)
		{
			perror("recv");
			exit(1);
		}
		strncpy(filename,buff,strlen(buff)>FILE_NAME_MAX_SIZE?FILE_NAME_MAX_SIZE:strlen(buff));

		printf("Preparing recv file : %s\n",filename);

		//recv file
		FILE *fd=fopen(filename,"wb+");
		if(NULL==fd)
		{
			perror("open");
			exit(1);
		}
		bzero(buff,BUFFSIZE);

		int length=0;
		while(length=recv(connfd,buff,BUFFSIZE,0))
		{
			if(length<0)
			{
				perror("recv");
				exit(1);
			}
			int writelen=fwrite(buff,sizeof(char),length,fd);
			if(writelen<length)
			{
				perror("write");
				exit(1);
			}
			bzero(buff,BUFFSIZE);
		}
		printf("Receieved file:%s finished!\n",filename);

		fclose(fd);
        close(connfd);
    }
    close(sockfd);
    return 0;
}
```

**<font color="black" size=5 face="仿宋">二、客户端程序：</font>**

```
/*
 * fileclient.c
 *
 *  Created on: Aug 11, 2017
 *      Author: wordzzzz
 */

#include "common.h"

int main(int argc, char **argv[])
{
    int clientfd;

    if(argc!=3)
    {
        fprintf(stderr,"Usage:./fileclient <IP_Address> <filename>\n");
        exit(1);
    }

    //Input the file name
    char filename[FILE_NAME_MAX_SIZE];
    bzero(filename,FILE_NAME_MAX_SIZE);
    strcpy(filename, argv[2]);

    struct sockaddr_in clientaddr;
    bzero(&clientaddr,sizeof(clientaddr));

    clientaddr.sin_family=AF_INET;
    clientaddr.sin_addr.s_addr=htons(INADDR_ANY);
    clientaddr.sin_port=htons(0);

    clientfd=socket(AF_INET,SOCK_STREAM,0);

    if(clientfd<0)
    {
        perror("socket");
        exit(1);
    }

    //bind
    if(bind(clientfd,(struct sockaddr*)&clientaddr,sizeof(clientaddr))<0)
    {
        perror("bind");
        exit(1);
    }

    struct sockaddr_in svraddr;
    bzero(&svraddr,sizeof(svraddr));
    if(inet_aton(argv[1],&svraddr.sin_addr)==0)
    {
        perror("inet_aton");
        exit(1);
    }
    svraddr.sin_family=AF_INET;
    svraddr.sin_port=htons(PORT);

    socklen_t svraddrlen=sizeof(svraddr);
    if(connect(clientfd,(struct sockaddr*)&svraddr,svraddrlen)<0)
    {
        perror("connect");
        exit(1);
    }

    //send file imformation
    char buff[BUFFSIZE];
    int count;
    bzero(buff,BUFFSIZE);
    strncpy(buff,filename,strlen(filename)>FILE_NAME_MAX_SIZE?FILE_NAME_MAX_SIZE:strlen(filename));
    count=send(clientfd,buff,BUFFSIZE,0);
    if(count<0)
    {
        perror("Send file information");
        exit(1);
    }

    //read file
    FILE *fd=fopen(filename,"rb");
    if(fd==NULL)
    {
        printf("File :%s not found!\n",filename);
    }
    else
    {
        bzero(buff,BUFFSIZE);
        int file_block_length=0;
        while((file_block_length=fread(buff,sizeof(char),BUFFSIZE,fd))>0)
        {
            printf("file_block_length:%d\n",file_block_length);
            if(send(clientfd,buff,file_block_length,0)<0)
            {
                perror("Send");
                exit(1);
            }
            bzero(buff,BUFFSIZE);
        }
        fclose(fd);
        printf("Transfer file finished !\n");
    }

    close(clientfd);
    return 0;
}
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**