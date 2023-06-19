# cdssp
1a.
%{
#include<stdio.h>
int c=0;
int w=0;
int s=0;
int l=0;
%}
%%
" " { s++; w++;}
[\n] { l++; w++;}
[\t\n] {w++;}
[^\t\n] {c++;}
%%
int yywrap()
{
return 1;
}
int main()
{
yyin=fopen("Info.txt", "r");
yylex();
printf("Characters = %d\nWords = %d\nSpaces = %d\nLines= %d\n",c,w,s,l);
return 0;
}
1b.
%{
#include<stdio.h>
int i=0;
%}
digit [0-9]
letter [a-z A-Z_]
%%
{letter}({letter}|{digit})* {i++;}
{digit}({letter}|{digit})* {i;}
%%
int main()
{
printf("Enter the values:\n");
yylex();
printf("Number of identifiers = %d\n", i);
return 0;
}
2a.
%{
#include<stdio.h>
int s=0,m=0;
%}
%%
"/*"[a-zA-Z0-9' '\t\n]*"*/" m++;
"//".* s++;
%%
void main(){
yyin=fopen("f1.txt","r");
yyout=fopen("f2.txt","w");
yylex();
fclose(yyin);
fclose(yyout);
printf("no of single line comments=%d\n",s);
printf("no of multi line comments=%d\n",m);
}
int yywrap()
{
return 1;
}
2b.
%{
#include<stdio.h>
int c=0;
%}
%%
[a-zA-Z]*[ ](and|or|but|yet|so)[ ][a-zA-Z]* {c=1;}
.|[\n];
%%
int yywrap()
{
return 1;
}
void main(){
printf("enter the text\n");
yylex();
if(c)
{
printf("The given statement is compound\n");
}
else
{
printf("The given statement is simple\n");
}
}
3a.
%{
#include<stdio.h>
int pi=0,ni=0,pf=0,nf=0;
%}
%%
[-][0-9]+ {ni++;}
[+]?[0-9]+ {pi++;}
[-][0-9]*\.[0-9]+ {nf++;}
[+]?[0-9]*\.[0-9]+ {pf++;}
%%
void main(int argc,char *argv[])
{
if(argc!=2)
{
printf("usage :./a.out in.txt \n");
exit(0);
}
yyin=fopen(argv[1],"r");
yylex();
printf("Number of positive integer %d\n",pi);
printf("Number of negative integer %d\n",ni);
printf("Number of positive fraction %d\n",pf);
printf("Number of negative fraction %d\n",nf);
}
int yywrap(){
return 1;
}
3b.
%{
#include<stdio.h>
int sf=0,pf=0;
%}
%%
"scanf" {sf++; fprintf(yyout,"readf");}
"printf" {pf++; fprintf(yyout,"writef");}
%%
int main()
{
yyin=fopen("f1.c","r");
yyout=fopen("f2.c","w");
yylex();
printf("no of scanf =%d\n no of printf =%d\n",sf,pf);
return 0;
}
4.
%{
#include "y.tab.h"
extern yylval;
%}
%%
[0-9]+ {yylval=atoi(yytext);return num;}
[\+\-\*\/] {return yytext[0];}
[)] {return yytext[0];}
[(] {return yytext[0];}
. {;}
\n {return 0;}
%%
//yacc code
%{#include<stdio.h>
#include<stdlib.h>
%}
%token num
%left '+''-'
%left '*''/'
%%
input:exp {printf("%d\n",$$);exit(0);}
exp:exp'+'exp {$$=$1+$3;}
|exp'-'exp {$$=$1-$3;}
|exp'*'exp {$$=$1*$3;}
|exp'/'exp {if($3==0){printf("Division by zero\n");exit(0);}
 else
$$=$1/$3;}
|'('exp')' {$$=$2;}
|num {$$=$1;};
%%
int yyerror()
{
printf("error");
exit(0);
}
int main()
{
printf("Enter the expression:\n");
yyparse();
}
5.%{
#include "y.tab.h"
%}
%%
[a-zA-z] return L;
[0-9] return D;
%%
//yacc code
%{
#include<stdio.h>
#include<stdlib.h>
%}
%token L D
%%
var:L E {printf("Valid Variable\n"); return 0;}
E:E L;
|E D;
| ;
%%
int main()
{
printf("Type the variable\n");
yyparse();
return 0;
}
int yyerror()
{
printf("Invalid Variable\n");
exit(0);
}
6.
%{
#include "y.tab.h"
%}
%%
a return A;
b return B;
. return yytext[0];
\n return yytext[0];
%%
//yacc code
%{
#include<stdio.h>
#include<stdlib.h>
%}
%token A B
%%
Str:S '\n' {return 0;}
S:A S B;
| ;
%%
int main()
{
printf("Type the string\n");
if (!yyparse())
printf("Valid String\n");
return 0;
}
int yyerror()
{
printf("Invalid String\n");
exit(0);
}
7.
#include<stdio.h>
#include<string.h>
#include<stdlib.h>
void main()
{
 char opcode[10], operand[10], label[10], code[10], mnemonic[3];
 int locctr, start, length;
 FILE *fp1,*fp2,*fp3,*fp4;
 fp1=fopen("Input.txt","r");
 fp2=fopen("Optab.txt","r");
 fp3=fopen("Symtabl.txt","w");
 fp4=fopen("Out.txt","w");
 fscanf(fp1,"%s\t%s\t%s", label,opcode,operand);
 if(strcmp(opcode,"START")==0)
 {
 start=atoi(operand);
 locctr=start;
 fprintf(fp4,"\t%s\t%s\t%s\n",label,opcode,operand);
 fscanf(fp1,"%s\t%s\t%s",label,opcode,operand);
 }
 else
 locctr=0;
 while(strcmp(opcode,"END")!=0)
 {
 fprintf(fp4,"%d\t",locctr);
 if(strcmp(label,"**")!=0)
 fprintf(fp3,"%s\t%d\n",label,locctr);
 fscanf(fp2,"%s\t%s",code,mnemonic);
 while(strcmp(code,"END")!=0)
 {
 if(strcmp(opcode,code)==0)
 {
 locctr+=3;
 break;
 }
 fscanf(fp2,"%s\t%s",code,mnemonic);
 }
 if(strcmp(opcode,"WORD")==0)
 locctr+=3;
 else if(strcmp(opcode,"RESW")==0)
 locctr+=(3*(atoi(operand)));
 else if(strcmp(opcode,"RESB")==0)
 locctr+=atoi(operand);
 else if(strcmp(opcode,"BYTE")==0)
 ++locctr;
 fprintf(fp4,"%s\t%s\t%s\t\n",label,opcode,operand);
 fscanf(fp1,"%s\t%s\t%s",label,opcode,operand);
 }
 fprintf(fp4,"%d\t%s\t%s\t%s\n",locctr,label,opcode,operand);
 length=locctr-start;
 printf("The length of the code:%d\n",length);
 fclose(fp1);
 fclose(fp2);
 fclose(fp3);
 fclose(fp4);
 }
 ![image](https://github.com/Nandini1230/cdssp/assets/72437179/21014214-ad17-4329-9e8d-7eb20841454b)

#include<stdio.h>
#include<string.h>
#include<stdlib.h>
void main()
{
FILE *fp;
int i,addrl,l,j,staddrl;
char name[10],line[50],namel[10],addr[10],rec[10],ch,staddr[10];
printf("enter program name:");
scanf("%s",name);
fp=fopen("abssrc.txt","r");
fscanf(fp,"%s",line);
for(i=2,j=0;i<8,j<6;i++,j++)
namel[j]=line[i];
namel[i]='\0';
printf("name from obj.%s\n",namel);
if(strcmp(name,namel)==0)
{
do{
fscanf(fp,"%s",line);
if(line[0]=='T'){
for(i=2,j=0;i<8,j<6;i++,j++)
staddr[j]=line[i];
staddr[j]='\0';
staddrl=atoi(staddr);
i=12;
while(line[i]!='$')
{
if(line[i]!='^')
{
printf("00%d\t%c%c\n",staddrl,line[i],line[i+1]);
staddrl++;
i=i+2;
}
else
i++;
}}
else if(line[0]='E')
fclose(fp);
}
while(!feof(fp));
}
}
9.
#include<stdio.h>
#include<ctype.h>
#include<stdlib.h>
void FIRST(char);
int count,n=0;
char prodn[10][10],first[10];
void main()
{
int i,choice;
char c,ch;
printf("Enter the number of productions: ");
scanf("%d",&count);
printf("Enter %d productions:\nEpsilon=$\n",count);
for(i=0;i<count;i++)
scanf("%s%c",prodn[i],&ch);
do{
n=0;
printf("Element :");
scanf("%c",&c);
FIRST(c);
printf("\nFIRST(%c)={",c);
for(i=0;i<n;i++)
printf(" %c",first[i]);
printf("}\n");
printf("Press 1 to continue :");
scanf("%d%c",&choice,&ch);
}
while(choice==1);
}
void FIRST(char c)
{
int j;
if(!(isupper(c)))first[n++]=c;
for(j=0;j<count;j++)
{
if(prodn[j][0]==c)
{
if(prodn[j][2]=='S')first[n++]='$';
else if(islower(prodn[j][2]))first[n++]=prodn[j][2];
else FIRST(prodn[j][2]);
}
}
}
10.
#include<stdio.h>
#include<string.h>
int k=0,z=0,i=0,j=0,c=0;
char a[16],ac[20],stk[15],act[10];
void check();
int main()
{ puts("GRAMMAR is E->E+E \n E->E*E \n E->(E) \n E->id");
puts("enter input string ");
gets(a);
c=strlen(a);
strcpy(act,"SHIFT->");
puts("stack \t input \t action");
printf("\n$\t%s$\t---",a);
for(k=0,i=0; j<c; k++,i++,j++)
{ if(a[j]=='i' && a[j+1]=='d')
{ stk[i]=a[j];
stk[i+1]=a[j+1];
stk[i+2]='\0';
a[j]=' ';
a[j+1]=' ';
printf("\n$%s\t%s$\t%sid",stk,a,act);
check();
}
else
{ stk[i]=a[j];
stk[i+1]='\0';
a[j]=' ';
printf("\n$%s\t%s$\t%ssymbols",stk,a,act);
check(); }
}
}
void check()
{
strcpy(ac,"REDUCE TO E");
for(z=0; z<c; z++)
if(stk[z]=='i' && stk[z+1]=='d')
{ stk[z]='E';
stk[z+1]='\0';
printf("\n$%s\t%s$\t%s",stk,a,ac);
j++;
}
for(z=0; z<c; z++)
if(stk[z]=='E' && stk[z+1]=='+' && stk[z+2]=='E')
{ stk[z]='E';
stk[z+1]='\0';
stk[z+2]='\0';
printf("\n$%s\t%s$\t%s",stk,a,ac);
i=i-2;
}
for(z=0; z<c; z++)
if(stk[z]=='E' && stk[z+1]=='*' && stk[z+2]=='E')
{ stk[z]='E';
stk[z+1]='\0';
stk[z+1]='\0';
printf("\n$%s\t%s$\t%s",stk,a,ac);
i=i-2;
}
for(z=0; z<c; z++)
if(stk[z]=='(' && stk[z+1]=='E' && stk[z+2]==')')
{ stk[z]='E';
stk[z+1]='\0';
stk[z+1]='\0';
printf("\n$%s\t%s$\t%s",stk,a,ac);
i=i-2;
} }

