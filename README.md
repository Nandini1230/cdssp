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
