Here's your C code properly formatted:

c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include <sys/socket.h>
#include <sys/types.h>
#include <netinet/in.h>

#define SERV_PORT 6349

int main(int argc, char **argv) {
    char filename[80], recvline[80];
    FILE *fp;
    struct sockaddr_in servaddr, cliaddr;
    int clilen, sockfd;

    sockfd = socket(AF_INET, SOCK_DGRAM, 0);
    bzero(&servaddr, sizeof(servaddr));
    servaddr.sin_family = AF_INET;
    servaddr.sin_port = htons(SERV_PORT);
    bind(sockfd, (struct sockaddr*)&servaddr, sizeof(servaddr));
    clilen = sizeof(cliaddr);
    
    recvfrom(sockfd, filename, 80, 0, (struct sockaddr*)&cliaddr, &clilen);
    
    printf("\n date in the file is \n");
    fp = fopen(filename, "r");
    while (fgets(recvline, 80, fp) != NULL) {
        printf("\n %s\n", recvline);
    }
    fclose(fp);
    
    return 0;
}


This version includes the necessary int return type for the main function, spaces around operators for better readability, and appropriate indentation


#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <unistd.h>

#define SERV_PORT 6349

int main(int argc, char **argv) {
    char filename[80];
    int sockfd;
    struct sockaddr_in servaddr;

    sockfd = socket(AF_INET, SOCK_DGRAM, 0);
    bzero(&servaddr, sizeof(servaddr));
    servaddr.sin_family = AF_INET;
    servaddr.sin_port = htons(SERV_PORT);
    inet_pton(AF_INET, argv[1], &servaddr.sin_addr);

    printf("Enter the file name: ");
    scanf("%s", filename);

    sendto(sockfd, filename, strlen(filename), 0, (struct sockaddr*)&servaddr, sizeof(servaddr));

    close(sockfd);
    return 0;
}






.
