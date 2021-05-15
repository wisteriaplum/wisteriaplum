- 👋 Hi, I’m @wisteriaplum


<!---
wisteriaplum/wisteriaplum is a ✨ special ✨ repository because its `CustomerBillingSystem.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->This is my project on Customer billing system that i did in my first year of college

	 

  

 #include<stdio.h>
 #include<conio.h>
 #include<stdlib.h>
#include<string.h>
 void input();
 void writefile();
 void search();
 void output();

 struct date{
	   int month;
	   int day;
	   int year;
	   };

  struct account {
	long long int number;
	char name[100];
	
	long int mobile_no;
	char street[100];
	char city[100];
	
	double oldbalance;
	double newbalance;
	double payment;
	struct date lastpayment;
  }customer;
  int tl,sl,ts;
 void main()
	{
	  int n1;int i;
	  char ch;
	  
	
	
	  
	  printf("   CUSTOMER BILLING SYSTEM:\n\n");
	
	  printf("===============================\n");
	  printf("\n1:    to add account on list\n");
	  printf("2:    to search customer account\n");
	  printf("3:    exit\n");
	  printf("\n================================\n");
	  do{
	       printf("\nselect what do you want to do?");
	       ch=getche();
	  }while(ch<='0' || ch>'3');
	  switch(ch){
		case '1':
			
			printf("\nhow many customer accounts?");
			scanf("%d",&n1);
			for(i=0;i<n1;i++){
				input();
				customer.newbalance=customer.oldbalance-customer.payment;
				writefile();
			}
			main();
		case '2':
			
			printf("search by what?\n");
			printf("\n1 --- search by customer number\n");
			printf("2 --- search by customer name\n");
			search();
			ch=getche();
			main();
		case '3':
			
			
			
			
			
			exit(1);
	  }
 }


   void input()
	{
	  FILE *fp=fopen("s.dat","rb");
	  fseek (fp,0,SEEK_END);
	  tl=ftell(fp);
	  sl=sizeof(customer);
	  ts=tl/sl;
	  fseek(fp,(ts-1)*sl,SEEK_SET);
	  fread(&customer,sizeof(customer),1,fp);
	  printf("\ncustomer account no:%d\n",++customer.number);
	 
	  
	  printf("\n       Name:");
	  scanf("%s",customer.name);
	  printf("\n       mobile no:");
	  scanf("%ld",&customer.mobile_no);
	  printf("         Street:");
	  scanf("%s",customer.street);
	  printf("         City:");
	  scanf("%s",customer.city);
	  printf("         Previous balance:");
	  scanf("%lf",&customer.oldbalance);
	 customer.payment=pay();
	  printf("         Payment date(mm/dd/yyyy):");
	  scanf("%d/%d/%d",&customer.lastpayment.month,&customer.lastpayment.day,&customer.lastpayment.year);
	  return;
   }

void pay()
{
	int n;int i;double p=0;
	printf("No. of  items purchased= \n");
	scanf("%d",&n);
	char name[n*100];double pri[n];int qu[n];double tot[n];
	for( i=0;i<n;i++)
	{	char it[20];int q;double pr;
		printf("Enter item:\n");
		scanf("%s",it);
		printf("Enter price:\n");
		scanf("%lf",&pr);
		printf("Enter quantity:\n");
		scanf("%d",&q);
		name[i]=it[0];
		pri[i]=pr;
		qu[i]=q;
		tot[i]=pr*q;
		p+=tot[i];
	}
	
	
	
	printf("------------------------------------------------------------------------------------------------------\n");
	printf("Item\tPrice\tQuantity\tTotal Price\n");
	for( i=0;i<n;i++)
	{
		printf("%c\t%lf\t%d\t%lf\n",name[i],pri[i],qu[i],tot[i]);
	}
	printf("\t\t\t\tTotal amount = Rs.%lf\n",p);
	printf("\t\t\t\tGST=5%\n");
	p=(1.05*p);
	printf("The TOTAL AMOUNT to pay inclusive of tax=%lf\n",p);
	printf("-----------------------------------------------------------------------------------------------------\n");
	printf("********************SUMMARY**********************\n");
}
   void writefile()
   {
	  FILE *fp;
	  fp=fopen("s.dat","ab");
	  fwrite(&customer,sizeof(customer),1,fp);
	  fclose(fp);
	  return;
   }

   void search()
   {
	 char ch;
	 char nam[100];
	 int n,i,m=1;
	 FILE *fp;
	 fp=fopen("s.dat","rb");
	 do{
		printf("\nenter your choice:");
		ch=getche();
	 }while(ch!='1' && ch!='2');
	 switch(ch){
	      case '1':
		    fseek(fp,0,SEEK_END);
		    tl=ftell(fp);
		    sl=sizeof(customer);
		    ts=tl/sl;
		    do{
			printf("\nchoose customer number:");
			scanf("%d",&n);
			if(n<=0 || n>ts)
			printf("\nenter correct\n");
			else{
			    fseek(fp,(n-1)*sl,SEEK_SET);
			    fread(&customer,sl,1,fp);
			    output();
			}
			printf("\n\nagain?(y/n)");
			ch=getche();
		    }while(ch=='y');
		    fclose(fp);
		    break;
	      case '2':
		    fseek(fp,0,SEEK_END);
		    tl=ftell(fp);
		    sl=sizeof(customer);
		    ts=tl/sl;
		    fseek(fp,(ts-1)*sl,SEEK_SET);
		    fread(&customer,sizeof(customer),1,fp);
		    n=customer.number;

		    do{
			printf("\nenter the name:");
			scanf("%s",nam);
			fseek(fp,0,SEEK_SET);
			for(i=1;i<=n;i++)
			{
			     fread(&customer,sizeof(customer),1,fp);
			     if(strcmp(customer.name,nam)==0)
			     {
				output();
				m=0;
				break;
			     }
			}
			if(m!=0)
			printf("\n\ndoesn't exist\n");
			printf("\nanother?(y/n)");
			ch=getche();
		    }while(ch=='y');
		    fclose(fp);
	      }
	      return;
	 }



   void output()
	 {
	   printf("\n\n    Customer no    :%d\n",customer.number);
	   printf("    Name 	   :%s\n",customer.name);
	   printf("    Mobile no      :%ld\n",customer.mobile_no);
	
	   printf("    Street         :%s\n",customer.street);
	   printf("    City           :%s\n",customer.city);
	   printf("    Old balance    :%lf\n",customer.oldbalance);
	   printf("    Current payment:%lf\n",customer.payment);
	   printf("    New balance    :%lf\n",customer.newbalance);
	   printf("    Payment date   :%d/%d/%d\n\n",customer.lastpayment.month,customer.lastpayment.day,customer.lastpayment.year);
	 
	  
	    
	      return;
	   }



























