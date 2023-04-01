 #include<stdio.h>
#include<conio.h>
#include<string.h>
#include<process.h>
#include<stdlib.h>
#include<direct.h>

struct contact
{
char name[20],veh[20],vehn[30];
}list;
char query[20], name[20];
FILE *fp, *ft;  
  
int i, n, ch, l, found;  
  
void add_vehicle()
{
    
system("cls");
fp=fopen("con.txt","a");
for (;;)
{ fflush(stdin);
printf("To exit enter blank space in the name input\nOwner Name (Use identical):");
scanf("%[^\n]",&list.name);
if(stricmp(list.name,"")==0 || stricmp(list.name," ")==0)
break;
fflush(stdin);
printf("vehicle name:");
scanf("%[^\n]",&list.vehn);
fflush(stdin);
printf("vehicle number :");
gets(list.veh);
printf("\n");
fwrite(&list,sizeof(list),1,fp);
}
fclose(fp);
}


void view_veh()
{
system("cls");
printf("LIST OF VEHICLE\n\t\t");
printf("Owner Name\t\t");
printf("Vehicle Name\t\t");
printf("Vehicle Number\n");

for( i=97;i<=122;i=i+1)
{

fp=fopen("con.txt","r");
fflush(stdin);
 found=0;
while(fread(&list,sizeof(list),1,fp)==1)
{
if(list.name[0]==i || list.name[0]==i-32)
{
printf("\nOwner Name\t: %s\nVehicle Name\t: %s\nVehicle Number\t: %s\n",list.name,list.vehn,list.veh);
found++;
}
}
if(found!=0)
{
    getch();

}
fclose(fp);

}

}


void search_veh()
{
system("cls");
do
{
 found=0;
printf("\n\n\t..::VEHICLE SEARCH\n\t===========================\n\t..::Name of vehicle number to search: ");
fflush(stdin);
scanf("%[^\n]",&query);
l=strlen(query);
fp=fopen("con.txt","r");

system("cls");
printf("\n\n..::Search result for '%s' \n===================================================\n",query);
while(fread(&list,sizeof(list),1,fp)==1)
{
for(i=0;i<=l;i++)
name[i]=list.name[i];
name[l]='\0';
if(stricmp(name,query)==0)
{
printf("\nOwner Name\t: %s\nVehicle Name\t: %s\nVehicle Number\t: %s\n",list.name,list.vehn,list.veh);
found++;
if (found%4==0)
{
printf("..::Press any key to continue...");
getch();
}
}
}

if(found==0)
printf("\n..::No match found!");
else
printf("\n..::%d match(s) found!",found);
fclose(fp);
printf("\n ..::Try again?\n\n\t[1] Yes\t\t[0] No\n\t");
scanf("%d",&ch);
}while(ch==1);
}

void delete_contact()
{
system("cls");
fflush(stdin);
printf("\n\n\t..::DELETE A VEHICLE RECORD\n\t==========================\n\t..::Enter the vehicle number to delete:");
scanf("%[^\n]",&name);
fp=fopen("con.txt","r");
ft=fopen("temp.dat","w");
while(fread(&list,sizeof(list),1,fp)!=0)
if (stricmp(name,list.name)!=0)
fwrite(&list,sizeof(list),1,ft);
fclose(fp);
fclose(ft);
remove("con.txt");
rename("temp.dat","con.txt");
}
