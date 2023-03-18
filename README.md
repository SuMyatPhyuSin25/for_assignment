# for_assignment

#include "stdio.h"
#include "stdlib.h"

#define SIZE 1000

struct trans{

    char note[200];
};

struct data{

    unsigned int id;
    char name[30];
    char nrc[50];
    char email[50];
    char password[50];
    char pOrb[10];//personal or business
    char loan_s[20];//loan status  //should loan or not
    unsigned int monthly_income;
    unsigned int loan_amount;//how much you take loan
    float loan_rate;
    char acc_s[10];//account status;
    int account_level;
    unsigned int phNumber;
    unsigned int current_amount;
    char address[100];
   unsigned int tranAmolimitPerday;//transationAmountLimitPerDay minimize for our project 5minutes
    struct trans trc[100];

};
struct data db[SIZE];


void loadingAlldataFromFile();
void printingAlldatafromFile();
void welcome();
void useractionsector();
int charcounting(char toCount[50]);
void emailvalidation(char tovalidate[50]);
void registration();
void StrCmp(char userInputchar[50]);
void comparingarray(char first[50],char second[50]);
void nrc_validatation(char nrc_check[50]);
void myStrongPassword(char strongpassword[50]);
void phonevalidation( unsigned int phone_tovalidate);
void myStringcopy(char first[50],char second[50]);
void recordingAlldatatoFile();
void login_form();
void finding_phone_number(unsigned int tofindphone);
void transfer_money(int transfer,int receiver,unsigned int amount_transfer);

int globalusers=0;
int gvalid=-1;
int emailExit=-1;
int two_charArray=-1;
int nrcvalid=-1;
int strongpass=-1;
int phonevalid=-1;
int phone_found=-1;
int useraction=-1;
int space_array[30];
void space_counter();

int main(){

space_counter();
loadingAlldataFromFile();
printingAlldatafromFile();
welcome();
while(useraction!=1){
    useractionsector();
}
    return 0;
}

void welcome(){

    int option=0;

    printf("\nWelcome to our bank!\n");
    printf("Press 1 to login\nPress 2 to register!\nPress 3 to exit!\n");
    scanf("%d",&option);

    if(option==1){

        login_form();

    }else if(option==2){


        registration();


    }else if(option==3){

        recordingAlldatatoFile();
        exit(1);


    }else{

        printf("This is invalid option.");
        welcome();
    }
}

void useractionsector(){

    int userOption=0;

       unsigned int phone=0;

     unsigned  int amount_to_transfer=0;



             printf("This is useraction sector!\n");

             printf("Press 1 to Transfer money!\nPress 2 to Withdraw!\nPress 3 to Update your information!\nPress 4 to Cash in!\nPress 5 to Take loan!\nPress 6 to View your history!\nPress 7 to Exit!\nEnter your option: ");

             scanf(" %d", &userOption);



    if(userOption==1){

        useraction=1;

        printf("This is transfer option!");

       phone_found=-1;
       while(phone_found==-1){

           printf("Enter receive phone number: ");

           scanf("%d",&phone);

           finding_phone_number(phone);

           if(phone_found==-1){

               printf("Your phone number doesn't belong to any name!");
           }


       }

       printf("Are you sure you want to send for %s email %s\n",db[phone_found].name,db[phone_found].email);



       while(amount_to_transfer<db[emailExit].current_amount){

           printf("Enter your amount to transfer: ");

           scanf("%u",&amount_to_transfer);


           if(db[emailExit].current_amount-1000>amount_to_transfer){

               break;
           }else{
               printf("No sufficient balance!");
               amount_to_transfer=0;
           }




       }

      two_charArray=-1;

       while(two_charArray==-1){

           char tran_pass[50];


            for(int count=0;count<3;count++){


                printf("Enter your password to proceed your trancsation: ");

                scanf(" %[^\n]",&tran_pass[0]);

                comparingarray(db[emailExit].password,tran_pass);


                if(two_charArray==-1){

                    printf("Wrong credential password!\n");



                }else{


                    transfer_money(emailExit, phone_found, amount_to_transfer);

                    printingAlldatafromFile();

                    useractionsector();

                    break;

                }
               if(count==2){

                   useractionsector();
               }


            }

       }


    }else if(userOption==7){

        recordingAlldatatoFile();
        welcome();

    }else{

        printf("Invalid option!");
    }
}

void transfer_money(int transfer,int receiver,unsigned int amount){

    printf("Loading to transfer......");

     db[transfer].current_amount=db[transfer].current_amount-amount;

     db[receiver].current_amount=db[receiver].current_amount+amount;

     printf("Transaction complete!");

}

void loadingAlldataFromFile(){

    FILE *fptr = fopen("bankpj.txt","r");

    if(fptr==NULL){

        printf("Error at loading all data from file!");

    }else{

        for(int ncc=0;ncc<SIZE;ncc++){

            fscanf(fptr, " %u%s%s%s%s%s%s%u%u%f%s%d%u%u%s%u\n",&db[ncc].id, &db[ncc].name, &db[ncc].nrc,&db[ncc].email,&db[ncc].password,&db[ncc].pOrb,&db[ncc].loan_s,&db[ncc].monthly_income,&db[ncc].loan_amount,&db[ncc].loan_rate,&db[ncc].acc_s,&db[ncc].account_level,&db[ncc].phNumber,&db[ncc].current_amount,&db[ncc].address,&db[ncc].tranAmolimitPerday);

            for(int gcc=0;gcc<space_array[ncc]-15;gcc++){

               fscanf(fptr,"%s",&db[ncc].trc[gcc].note[0]);
            }


            if(db[ncc].email[0]=='\0'){

                break;
            }
            globalusers++;
        }
        fclose(fptr);

    }

}

void printingAlldatafromFile(){

    for(int ncc=0;ncc<globalusers;ncc++){

        printf("  %u-%s-%s-%s-%s-%s-%s-%u-%u-%f-%s-%d-%u-%u-%s-%u", db[ncc].id,db[ncc].name,db[ncc].nrc,db[ncc].email,db[ncc].password,db[ncc].pOrb,db[ncc].loan_s,db[ncc].monthly_income,db[ncc].loan_amount,db[ncc].loan_rate,db[ncc].acc_s,db[ncc].account_level,db[ncc].phNumber,db[ncc].current_amount,db[ncc].address,db[ncc].tranAmolimitPerday);

        for(int gcc=0;gcc<space_array[ncc]-15;gcc++){

            printf("-%s",&db[ncc].trc[gcc].note[0]);
        }
        printf("\n");

    }

}

void space_counter(){

    FILE *fptr=fopen("bankpj.txt","r");

    if(fptr==NULL){

        printf("Error at space_counter!");
    }else{

    char c=  fgetc(fptr);

    int index=0;

    while(!feof(fptr)){
         if(c!='\n'){

             if(c==' '){

                 space_array[index]+=1;
             }
             c=fgetc(fptr);
         }else{

             index++;
             c=fgetc(fptr);
         }

    }
    for(int aaa=0;aaa<30;aaa++){

        printf("%d",space_array[aaa]);

        printf(" ");
    }
    printf("\n");

    }
}
void registration(){

    char reEmail[50];

    char reName[30];

    char reNrc[50];

    char rePassword[50];

    char re_Address[100];

   unsigned long long int re_Phone=0;

    unsigned int Re_current_amount;

    printf("This is Bank Registration!\n");

    printf("Enter your email to register: ");

    scanf(" %[^\n]",&reEmail);

    gvalid=-1;

    emailvalidation(reEmail);

    if(gvalid!=-1){

        emailExit=-1;

        StrCmp(reEmail);

        if(emailExit==-1){

            printf("Your email is valid!\n");

            printf("Enter your name: ");

            scanf(" %[^\n]",&reName[0]);

            nrcvalid=-1;

            while(nrcvalid==-1) {



                printf("Enter your NRC number: ");

                scanf(" %[^\n]",&reNrc[0]);

                nrcvalid=-1;

                nrc_validatation(reNrc);

              if(nrcvalid==-1){

                  printf("Your NRC is not valid now!");

              }


            }


               printf("Your NRC is passed in!");
            strongpass=-1;

             while(strongpass==-1){

                 printf("Enter your password!");

                 scanf(" %[^\n]",&rePassword[0]);


                 strongpass=-1;
                 myStrongPassword(rePassword);

                 if(strongpass==-1){
                     printf("Your password is not valid!");
                 }

             }

             printf("Your password is valid and saved!");


             phonevalid=-1;

             while(phonevalid==-1){

                 printf("Enter your phone number: ");

                 scanf("%u",&re_Phone);

                 phonevalidation(re_Phone);

                  if(phonevalid==-1){

                      printf("Your phone number is not valid or is already exit!");
                  }


             }



             printf("Your phone number is saved!\n");

             printf("Enter your current amount: ");

             scanf(" %u",&Re_current_amount);

             printf("Enter your monthly income: ");

             scanf(" %u",&db[globalusers].monthly_income);

             printf("Enter your address: ");

             scanf(" %[^\n]",&re_Address[0]);

             printf("Enter your trc note: ");

             scanf(" %[^\n]",&db[globalusers].trc[0].note[0]);

             myStringcopy(db[globalusers].email,reEmail);

             myStringcopy(db[globalusers].name,reName);

             myStringcopy(db[globalusers].nrc,reNrc);

             myStringcopy(db[globalusers].password,rePassword);


             myStringcopy(db[globalusers].pOrb,db[2].pOrb);

             myStringcopy(db[globalusers].loan_s,db[2].loan_s);

             myStringcopy(db[globalusers].acc_s,db[2].acc_s);


             db[globalusers].loan_amount=db[2].loan_amount;

             db[globalusers].loan_rate=db[2].loan_rate;

             db[globalusers].account_level=db[2].account_level;

             db[globalusers].tranAmolimitPerday=db[2].tranAmolimitPerday;

             db[globalusers].id=globalusers+1;

             db[globalusers].phNumber=re_Phone;

            myStringcopy(db[globalusers].address,re_Address);

            db[globalusers].current_amount=Re_current_amount;

             globalusers++;

             printingAlldatafromFile();

             welcome();


        }else{

            printf("Your email was already exit!");



        }


    }else{

        printf("Your gmail is not valid!\n");
        registration();
    }


}

int charcounting(char toCount[50]){

    int charcount=0;

    for(int i=0;i<50;i++){

        if(toCount[i]=='\0'){

            break;
        }else{

            charcount++;

        }
    }
    return charcount;
}

void emailvalidation(char tovalidate[50]){

    int count=0;

  int validate=  charcounting(tovalidate);

  char form[10]={'@','g','m','a','i','l','.','c','o','m'};

  for(int i=0;i<validate;i++){

      if(tovalidate[i]==' '){

          break;
      }else if(tovalidate[i]=='@'){
          break;
      }else{

          count++;
      }



  }

  int check=0;

  for(int ncc=0;ncc<10;ncc++){

      if(tovalidate[count]!=form[ncc]){

          break;
      }else{

          check++;
          count++;
      }
  }
  if(check==10){
      gvalid=1;
  }


}

void StrCmp(char userInputchar[50]){

    int samecount=0;


    int second=charcounting(userInputchar);

    for(int j=0;j<globalusers;j++){

        int first=charcounting(db[j].email);

        if(first==second){

            for(int gcc=0;gcc<first;gcc++){

                if(db[j].email[gcc]==userInputchar[gcc]){

                    samecount++;

                }else{

                    break;
                }
            }

        }
        if(second==samecount){

            emailExit=j;

            break;
        }



    }



}

struct nrc_region{

    char nrc_regioncheck[10];
};

struct nrc_region nrcRegionchecking[3];

void nrc_validatation(char nrc_check[50]){



   int nrcCount=  charcounting(nrc_check);

   int nrc_char=0;

   for(int i=0;i<nrcCount;i++){

       if(nrc_check[i]==')'){

           break;
       }
       nrc_char++;
   }
   for(int gcc=0;gcc<3;gcc++){

       two_charArray=-1;

       comparingarray(nrc_check,db[gcc].nrc);

       if(two_charArray==1){

           nrcvalid=1;

           break;
       }
   }



}

void comparingarray(char first[50],char second[50]){

int firstCount=charcounting(first);

int secondCount=charcounting(second);

int nrc_Count=0;

if(firstCount==secondCount){

    for(int ncc=0;ncc<firstCount;ncc++){

        if(first[ncc]!=second[ncc]){

            break;
        }
        nrc_Count++;
    }

    if(nrc_Count==secondCount){

        two_charArray=1;


    }
}





}

void myStrongPassword(char strongpassword[50]){

    int i=0;

    int strongpassone=0;

    int strongpasstwo=0;

    int strongpassthree=0;

    int strongpassfour=0;

  int pass_counter=  charcounting(strongpassword);

   if(pass_counter==7){

       while(strongpass==-1){


               if(i==pass_counter){

                   strongpass=-1;

                   break;
               }

               if(strongpassword[i]>=33 && strongpassword[i]<=42){

                   strongpassone++;
               }else if(strongpassword[i]>=42 && strongpassword[i]<=57){

                   strongpasstwo++;
               }else if(strongpassword[i]>=65 && strongpassword[i]<=90){

                   strongpassthree++;
               }else if(strongpassword[i]>=97 && strongpassword[i]<=122){

                   strongpassfour++;
               }
               i++;


               if(strongpassone>0 && strongpasstwo>0 && strongpassthree>0 && strongpassfour>0){

                   strongpass=1;
               }


       }





   }else{

       printf("We need at least 7 characters!");
       strongpass=-1;
   }


}

void phonevalidation(unsigned int phone_tovalidate){

    int phone_counter=0;

    for(int i=0;i<globalusers;i++){

        if(phone_tovalidate!=db[i].phNumber){
            phone_counter++;
        }else{
            phonevalid=-1;
        }
    }
    if(phone_counter==globalusers){
        phonevalid=1;
    }

}

void myStringcopy(char first[50],char second[50]){

  int copy_counter=  charcounting(second);

  for(int i=0;i<50;i++){

      first[i]='\0';
  }
  for(int j=0;j<copy_counter;j++){

      first[j]=second[j];
  }
}
void recordingAlldatatoFile() {

    FILE *fptr = fopen("bankpj.txt", "w");

    if (fptr == NULL) {

        printf("Error at recording all data to file!");
    } else {

        for (int ncc = 0; ncc < globalusers; ncc++) {
            fprintf(fptr, "%u%c%s%c%s%c%s%c%s%c%s%c%s%c%u%c%u%c%f%c%s%c%d%c%u%c%u%c%s%c%u", db[ncc].id, ' ',
                    db[ncc].name, ' ', db[ncc].nrc, ' ', db[ncc].email,
                    ' ', db[ncc].password, ' ', db[ncc].pOrb, ' ', db[ncc].loan_s, ' ', db[ncc].monthly_income, ' ',
                    db[ncc].loan_amount, ' ', db[ncc].loan_rate, ' ', db[ncc].acc_s, ' ', db[ncc].account_level, ' ',
                    db[ncc].phNumber, ' ', db[ncc].current_amount, ' ', db[ncc].address, ' ',
                    db[ncc].tranAmolimitPerday);

           for(int gcc=0;gcc<space_array[ncc]-15;gcc++){

                fprintf(fptr," %s",&db[ncc].trc[gcc].note[0]);
            }
            fprintf(fptr,"%c","\n");


        }
    }
    printf("Recording all data to bankpj.txt is complete!");

    fclose(fptr);
}

void login_form(){
    char lEmail[50];

    char lPassword[50];

    emailExit=-1;

    two_charArray=-1;

  while(emailExit==-1 || two_charArray==-1){

      printf("Enter your email to login: ");

      scanf(" %[^\n]",&lEmail[0]);

      printf("Enter your password: ");

      scanf(" %[^\n]",&lPassword[0]);

      StrCmp(lEmail);

      comparingarray(db[emailExit].password,lPassword);


      if(emailExit==-1 || two_charArray==-1){

          emailExit=-1;

          two_charArray=-1;

          printf("Your login credential was wrong!");
      }

  }
  printf(" Welcome Mr/Mrs! %s",db[emailExit].name);

  useractionsector();




}
void finding_phone_number(unsigned int tofindphone){

    for(int i=0;i<globalusers;i++){

        if(db[i].phNumber==tofindphone){

            phone_found=i;
            break;
        }
    }
}


