# Linux-IPC-Shared-memory
Ex06-Linux IPC-Shared-memory

# AIM:
To Write a C program that illustrates two processes communicating using shared memory.

# DESIGN STEPS:

### Step 1:

Navigate to any Linux environment installed on the system or installed inside a virtual environment like virtual box/vmware or online linux JSLinux (https://bellard.org/jslinux/vm.html?url=alpine-x86.cfg&mem=192) or docker.

### Step 2:

Write the C Program using Linux Process API - Shared Memory

### Step 3:

Execute the C Program for the desired output. 

# PROGRAM:

## Write a C program that illustrates two processes communicating using shared memory.

```
//shmry1.c

#include<unistd.h> 
#include<stdlib.h> 
#include<stdio.h> 
#include<string.h> 
#include<sys/shm.h>
#define TEXT_SZ 2048 
struct shared_use_st
{
int written_by_you;
char some_text[TEXT_SZ];
};
int main()
{
int running = 1;
void *shared_memory = (void *)0; 
struct shared_use_st *shared_stuff; 
int shmid;
srand( (unsigned int)getpid() ); 
shmid = shmget( (key_t)1234, sizeof(struct shared_use_st), 0666 |IPC_CREAT );
printf("Shared memory id is %d \n",shmid);
if (shmid == -1)
{
fprintf(stderr, "shmget failed\n");
exit(EXIT_FAILURE);
}
shared_memory = shmat(shmid,(void *)0, 0); if (shared_memory == (void *)-1)
{
fprintf(stderr,	"shmat	failed\n"); exit(EXIT_FAILURE);
}
printf("Memory Attached at %x\n", (int)shared_memory);
shared_stuff = (struct shared_use_st *) shared_memory;
shared_stuff->written_by_you	=	0;
 while(running)
{
if(shared_stuff->written_by_you)
{
printf("You Wrote: %s", shared_stuff->some_text);
sleep( rand() %4 );
shared_stuff->written_by_you = 0;
if (strncmp(shared_stuff->some_text, "end", 3)== 0){
running = 0;}}
}
if (shmdt(shared_memory) == -1)

{
fprintf(stderr, "shmdt failed\n"); exit(EXIT_FAILURE);
}
if (shmctl(shmid, IPC_RMID, 0) == -1)
{
fprintf(stderr, "failed to delete\n");
exit(EXIT_FAILURE);
} exit(EXIT_SUCCESS);
}
```

```
//shmry2.c

#include<unistd.h> 
#include<stdlib.h> 
#include<stdio.h> 
#include<string.h>
#include<sys/shm.h>
#define TEXT_SZ 2048 
struct shared_use_st{
int written_by_you;
char some_text[TEXT_SZ];
};
int main()
{
int running =1;
void *shared_memory = (void *)0; 
struct shared_use_st *shared_stuff; 
char buffer[BUFSIZ];
int shmid;
shmid	=shmget(	(key_t)1234,	sizeof(struct shared_use_st), 0666 | IPC_CREAT);
printf("Shared memory id = %d \n",shmid);
if (shmid == -1)
{
fprintf(stderr, "shmget failed\n"); exit(EXIT_FAILURE);
}
shared_memory=shmat(shmid, (void *)0, 0);
if (shared_memory == (void *)-1){
fprintf(stderr,	"shmat	failed\n"); exit(EXIT_FAILURE);}
printf("Memory Attached at %x\n", (int) shared_memory); 
shared_stuff = (struct shared_use_st *)shared_memory; 
while(running)
{
while(shared_stuff->written_by_you== 1)
{
sleep(1);
printf("waiting for client.	\n");
}
printf("Enter Some Text: "); fgets (buffer, BUFSIZ, stdin);
strncpy(shared_stuff->some_text, buffer, TEXT_SZ);
shared_stuff->written_by_you = 1;
if(strncmp(buffer, "end", 3) == 0){
running = 0;}}
if (shmdt(shared_memory) == -1)
{
fprintf(stderr, "shmdt failed\n"); exit(EXIT_FAILURE);
} exit(EXIT_SUCCESS);
}
```


## OUTPUT
![6i](https://github.com/user-attachments/assets/d93dd002-c0a2-4ee0-9b3a-233d998fd735)
![6ii](https://github.com/user-attachments/assets/1474bab4-15b0-465d-979f-2b7f39f81f5c)


![6iii](https://github.com/user-attachments/assets/9155fa01-0357-4b09-9e2f-4aa022223075)
![6iv](https://github.com/user-attachments/assets/88589cd8-62c9-4d18-b543-97d96c661cf1)

![6v](https://github.com/user-attachments/assets/07498d33-669d-4799-88c5-b1172aed218d)




# RESULT:
The program is executed successfully.
