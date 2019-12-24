# shell
#include <stdio.h>
#include <unistd.h>
#include <sys/wait.h>
#include <stdlib.h>
#include <string.h>
#include <limits.h>
#define LSH_TOK_DELIM " \t\r\n\a" //space-tab-carriage return-new line-alert(bell)
char* line(){
   char* line=NULL;
   ssize_t buffer = 0;
   getline(&line, &buffer,stdin); //if we want to read lines directly from the file as string, we use getline(). *Here’s a typical getline() statement: getline(&buffer,&size,stdin); *&buffer is the address of the first character position where the input string will be stored. It’s not the base address of the buffer, but of the first character in the buffer. *&size is the address of the variable that holds the size of the input buffer, another pointer. *stdin is the input file handle. So you could use getline() to read a line of text from a file, but when stdin is specified, standard input is read.
   return line;
}
char** splitLine(char* lsline){
	char **args = malloc(sizeof(char*)*50);
	char *arg;
	
	
	int j=0;
	arg=strtok(lsline, LSH_TOK_DELIM); //Divides line into arguments according to those specified in LSH_TOK_DELIM

	while(arg!=NULL){
		args[j]=arg;
		j++;
		arg=strtok(NULL, LSH_TOK_DELIM);
	}
	return args;
		
	
}
int launch(char** args){
	pid_t childProcess, process; //the process that creates the new process is called the parent process, and the new one is called the child process and IDs of processes are of type pid_t.
	int status;
	char *cmdargs[]={
		"/bin/bash",
		"./shells/backup.sh", //The backup.sh command takes 2 arguments along with
		args[1],
		NULL};
	
	char *cmdargs2[]={
		"./c_files/switch", //takes 3 arguments with switch command
		args[1],args[2],
		NULL};
		
      if(strcmp(args[0],"switch")==0){
		  childProcess=fork();  //fork() creates a copy of the process currently running. The new process here is the child process.
		  if(childProcess==0){  //this part works in child process
		 
		execv(cmdargs2[0],cmdargs2); //execv() allows you to call a different program in the computer's file system from the C program we have written
		
	}else if(childProcess>0){
		if((process=wait(&status))<0){ //It is undesirable for a parent process to complete and close before its own child process. For this reason, there is a wait (& donus_deg) command that can be used by the parent to avoid such situations.
			_exit(1);
	    }
	}
}
		if(strcmp(args[0],"cd")==0){
			chdir(args[1]);      //The chdir command is a system function (system call) which is used to change the current working directory
		}



		if(strcmp(args[0],"backup")==0){
		  childProcess=fork();
		  if(childProcess==0){
		 
		execvp(cmdargs[0],cmdargs);
		
	}else if(childProcess>0){
		if((process=wait(&status))<0){
			_exit(1);
	    }
	}
}
	     if(strcmp(args[0],"exit")==0){
		return 1;
	    }
	return 0;
}


int main(){
  int state=0;
  char* lsline;
  char** args;
  char cwd[PATH_MAX];
  while(state==0){
	  getcwd(cwd,sizeof(cwd)); //The getcwd() function returns the name of the current working directory.
	  printf("\n%s->",cwd);
  lsline=line();
  args= splitLine(lsline);
  
  state=launch(args);
  free(lsline); //With Malloc, you release a block that you have received with the free function, saying "I'm done with it"


  free(args);
  }
  return 0;
  
}
  
