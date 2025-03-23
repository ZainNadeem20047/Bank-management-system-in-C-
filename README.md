# Bank-management-system-in-C++
#include <iostream>
#include<conio.h>
#include<fstream>

using namespace std;

// Function Prototypes
 void intromenu();
 void displaymenu();
 void managermenu();
 void manageroption();
 void adduser();
 void deleteuser();
 void showaccounts();
 void displayUserInfo();
 void userlogin();
 void usermenu();
 void useroption();
 void storeinarray();
 void useroption();
 
 //structures here
 struct addusers
 {
 	string name,fathername,cnic,phnumber,adress,line= "----------------------------";
 	int pass;
	float balance; 	
 };
 


int main()
{
	intromenu();	
	displaymenu();
}

//intro menu
void intromenu(){
	cout << "----------------------"<<endl;      
	cout << "Bank Management System"<<endl;
	cout << "----------------------"<<endl;
	cout << "Submitted By:"<<endl;
	cout << "Muhammad Farooq Naeem"<<endl;
	cout << "Sp23-BCS-036"<<endl;
	cout << "Muhammad Ehsan Mumtaz"<<endl;	
	cout << "Sp23-BCS-039"<<endl;
	cout << "----------------------"<<endl;
	cout << "Press any key to continue";
	getch();
    

}

//add new user or client
void adduser()
{
	system("cls");
	ofstream  obj;
	obj.open("employee.txt",ios::app);
	int size;
	cout<<"How many Users you want to add"<<endl;                	
	cin>>size;
	
	addusers user[size];
	for(int i=0;i<size;i++)
	{
		cin.ignore();
		cout<<"Enter the name of user:"<<i+1<<endl;
		getline(cin,user[i].name);
		obj<<user[i].name<<endl;
		
		
		cout<<"Enter father name"<<endl;
		getline(cin,user[i].fathername);
		obj<<user[i].fathername<<endl;
		
		
		cout<<"Enter user's Cnic"<<endl;
		getline(cin,user[i].cnic);
		obj<<user[i].cnic<<endl;
		

		cout<<"Enter user Phone Number"<<endl;
		getline(cin,user[i].phnumber);
		obj<<user[i].phnumber<<endl;
		
		
		cout<<"Enter user adress"<<endl;
		getline(cin,user[i].adress);
		obj<<user[i].adress<<endl;
		
		cout<<"Enter the Password you want to assign:"<<endl;
		cin>>user[i].pass;
		obj<<user[i].pass<<endl;
		cin.ignore();
		
		cout<<"How much amount user want to deposit"<<endl;
		cin>>user[i].balance;
		obj<<user[i].balance<<endl;
		obj<<user[i].line<<endl;
	}
	obj.close();
	cout<<"\n Account/s created successfully"<<endl;
	cout<<"Press any key to go back to Manager Menu"<<endl;
	getch();
	manageroption();
}

//function for viewing account
void showaccounts()
{
	ifstream obj;
	obj.open("employee.txt");
	string line;
	while(!obj.eof())
	{
		getline(obj,line);
		cout<<line<<endl;
	}
	obj.close();
	cout<<"\nThese are all Accounts in the bank"<<endl;
	cout<<"Press any key to Go back to Manager menu"<<endl;
	getch();
	manageroption();
	
}

// Function definition for deleting a user
void deleteuser() {
    system("cls");
    
    // Prompt the user for the name to delete
    string nameToDelete,cnictodelete;
    cin.ignore();
    cout << "Enter the name of the user to delete:";
    getline(cin, nameToDelete);
    cout <<"Enter the cnic of the user to delete:";
    getline(cin, cnictodelete);
    

    ifstream infile("employee.txt");
    ofstream outfile("temp.txt");

    if (!infile.is_open()) {
        cout << "Error: Could not open file for reading." << endl;
        getch();
        return;
    }

    if (!outfile.is_open()) {
        cout << "Error: Could not open file for writing." << endl;
        getch();
        return;
    }

   //string name,fathername,cnic,phnumber,adress,pass,balance,line;
   addusers user;
   bool found = false;

    while (getline(infile, user.name)) {
        // Read the rest of the user data
        getline(infile, user.fathername);
        getline(infile, user.cnic);
        getline(infile, user.phnumber);
        getline(infile, user.adress);
        infile >>user.pass;
        infile >>user.balance;
        infile.ignore();
        getline(infile, user.line);


        // Check if the current user's name matches the one to delete
        if (user.name != nameToDelete || user.cnic != cnictodelete) {
            // Write the user to the temp file if it's not the one to delete
            outfile << user.name << endl;
            outfile << user.fathername << endl;
            outfile << user.cnic << endl;
            outfile << user.phnumber << endl;
            outfile << user.adress << endl;
            outfile << user.pass << endl;
            outfile << user.balance <<endl;
            outfile << user.line <<endl;
            found = false;
        } else if(user.name == nameToDelete && user.cnic == cnictodelete) {
            found = true;
        }
    }

    infile.close();
    outfile.close();

    remove("employee.txt");
    rename("temp.txt", "employee.txt");
    if (found) {
        cout << "User '" << nameToDelete << " with cnic "<<cnictodelete<<"' deleted successfully." << endl;

    } 
    else if (!found)
    {
	
        cout << "User '" << nameToDelete <<" with cnic "<<cnictodelete<<"' not found." << endl;
    }

    cout << "Press any key to go back to Manager Menu." << endl;
    getch();
    manageroption();
}


//Function definition for displaying user information
void displayUserInfo() {
    system("cls");

    // Prompt the user for the name and CNIC to display information
    string nameToDisplay;
    string cnicToDisplay;
    cout << "Enter the name of the user to display information: ";
    cin.ignore();
    getline(cin, nameToDisplay);
    cout << "Enter the CNIC of the user to display information: ";
    getline(cin, cnicToDisplay);

    ifstream infile("employee.txt");

    if (!infile.is_open()) {
        cout << "Error: Could not open file for reading." << endl;
        getch();
        return;
    }

    addusers user;
    bool found = false;

    while (getline(infile, user.name)) {
        // Read the rest of the user data
        getline(infile, user.fathername);
        getline(infile, user.cnic);
        getline(infile, user.phnumber);
        getline(infile, user.adress);
        infile >> user.pass;
        infile >> user.balance;
        infile.ignore(); // clear the newline character from the buffer
        getline(infile, user.line);
        

        // Check if the current user's name and CNIC match the ones to display
        if (user.name == nameToDisplay && user.cnic == cnicToDisplay) {
            found = true;

            // Display the user information
            cout << "Name: " << user.name << endl;
            cout << "Father's Name: " << user.fathername << endl;
            cout << "CNIC: " << user.cnic << endl;
            cout << "Phone Number: " << user.phnumber << endl;
            cout << "Address: " << user.adress << endl;
            cout << "Password:" << user.pass << endl;
            cout << "Balance: " << user.balance << endl;
            cout<<user.line<<endl;

            break; // Exit the loop once the user is found
        }
    
    }

    infile.close();

    if (!found) {
        cout << "User '" << nameToDisplay << "' with CNIC '" << cnicToDisplay << "' not found." << endl;
        found= false;
    }

    cout << "Press any key to go back to Manager Menu." << endl;
    getch();
    manageroption();
}


//manager option or chice pannel
void manageroption()
{
	system("cls");
	int choice;
	cout<<"----------------------------------"<<endl;
	cout<<"1.Create a New Account"<<endl;
	cout<<"2.View Accounts info"<<endl;
	cout<<"3.Delete a Account"<<endl;
	cout<<"4.Search any user by name and cnic"<<endl;
	cout<<"5.Back to Main Menu"<<endl;
	cout<<"----------------------------------"<<endl;
	cin>>choice;
	switch(choice)
	{
		case 1:
			adduser();
			break;
		case 2:
			showaccounts();
			break;
		case 3:
			deleteuser();
			break;
		case 4:
			displayUserInfo();
			break;
		case 5:
			displaymenu();
			break;
		default:
			cout<<"Invalid Choice...Press any key"<<endl;
			getch();
			manageroption();			
	}
}

//manager menu
void managermenu()
{
	system("cls");
	int pass,choice;
	cout<<"-------------"<<endl;
	cout<<"Manager Login"<<endl;
	cout<<"-------------"<<endl;
	cout<<"1.Enter Your Password"<<endl;
	cout<<"2.Back to main menu"<<endl;
	cout<<"Enter your Choice"<<endl;
	cin>>choice;
	switch(choice)
	{
		case 1:
		cout<<"-----------------"<<endl;
		cout<<"Enter 4 digit Pin"<<endl;
		cout<<"-----------------"<<endl;
		cin>>pass;
		if(pass==3214)
		{
			manageroption();
		}
		else
		{
		cout<<"\n\n";
		cout<<"Invalid Password...Please Try Again"<<endl;
		cout<<"Press Enter to continue"<<endl;
		getch();
		managermenu();
		}
		break;
		case 2:
		displaymenu();
		break;
		default:
		cout<<"Invalid Entry \nPress any key";
		getch();
		managermenu();
	}
		
}

void useroption(addusers arr[])
{
	system("cls");
	int choice;
	cout <<"-------------------"<<endl;
	cout <<"1.View account Info"<<endl;
	cout <<"2.View Balance"<<endl;
	cout <<"3.Bill Payment"<<endl;
	cout <<"4.Deposit Cash"<<endl;
	cout <<"5.Cash withdraw"<<endl;
	cout <<"6.Exit"<<endl;
	cout <<"-------------------"<<endl;
	cout <<"Make your choice"<<endl;
	cin >> choice;
	
	switch(choice)
	{
		case 1:
			{
				system("cls");
				cout <<"------------------"<<endl;
				cout <<"Your Account Info:"<<endl;
				cout <<"------------------"<<endl;
				cout <<"Name: "<<arr[0].name<<endl;
				cout <<"Fathername: "<<arr[0].fathername<<endl;
				cout <<"CNIC: "<<arr[0].cnic<<endl;
				cout <<"Phone-Number:"<<arr[0].phnumber<<endl;
				cout <<"Address: "<<arr[0].adress<<endl;
				cout <<"Password: "<<arr[0].pass<<endl;
				cout <<"Balance: "<<arr[0].balance<<endl;
				cout <<arr[0].line<<endl;
				cout<<"Press any key to continue"<<endl;
				getch();
			}
			useroption(arr);
			break;
		case 2:
			{
				system("cls");
				cout <<"-----------------------"<<endl;
				cout <<"Your Current Balace is: ";
				cout <<arr[0].balance<<" Rs only."<<endl;
				cout <<"------------------------"<<endl;
				cout<<"Press any key to continue"<<endl;
				getch();
			}
			useroption(arr);
			break;
		case 3:
			{
				system("cls");
				cin.ignore();
				string refnum; 
				float bill;
				
				cout <<"Enter 14-digit Bill reference number:";
				getline(cin, refnum);
				cout <<"Enter amount:";
				cin >>bill;
				
				if(bill > 0)           
				{
					if(arr[0].balance > bill)
					{
						arr[0].balance=arr[0].balance-bill;
						cout<<"Bill paid successfully! "<<endl;	
					}
					else
					{
						cout<<"Insufficient Balance! "<<endl;
					}
				}
				else
				{
					cout<<"Invalid Input"<<endl;	
				}

					ifstream infile;
					ofstream outfile;
					
					infile.open("employee.txt");
					outfile.open("temp.txt");
					
					if(!infile.is_open())
					{
						cout<<"Error: could not open file for reading."<<endl;
						getch();
						return;
					}
					
					if(!outfile.is_open())
					{
						cout<<"Error: could not open file for reading."<<endl;
						getch();
						return;
					}
					
					addusers user;
					bool found=false;
					
					while(!infile.eof())
					{
						while (getline(infile, user.name)) 
						{
					        // Read the rest of the user data
					        getline(infile, user.fathername);
					        getline(infile, user.cnic);
					        getline(infile, user.phnumber);
					        getline(infile, user.adress);
					        infile >>user.pass;
					        infile >>user.balance;
					        infile.ignore();
					        getline(infile, user.line);
					
				
				        	// Check if the current user's name matches the one to delete
				        	if (user.name != arr[0].name|| user.pass != arr[0].pass) 
							{
					            // Write the user to the temp file if it's not the one to delete
					            outfile << user.name << endl;
					            outfile << user.fathername << endl;
					            outfile << user.cnic << endl;
					            outfile << user.phnumber << endl;
					            outfile << user.adress << endl;
					            outfile << user.pass << endl;
					            outfile << user.balance <<endl;
					            outfile << user.line <<endl;
					            found = false;
				        	} 
							else if(user.name == arr[0].name && user.pass == arr[0].pass) 
							{
				            	found = true;
				        	}
				    	}
					}
				    
				    infile.close();
				    outfile.close();
				    
				    remove("employee.txt");
				    rename("temp.txt","employee.txt");
					
					ofstream obj;
					obj.open("employee.txt", ios::app);
					obj <<arr[0].name<<endl;
					obj <<arr[0].fathername<<endl;
					obj <<arr[0].cnic<<endl;
					obj <<arr[0].phnumber<<endl;
					obj <<arr[0].adress<<endl;
					obj <<arr[0].pass<<endl;
					obj <<arr[0].balance<<endl;
					obj <<arr[0].line<<endl;
					obj.close();
					cout<<"Press any key to continue"<<endl;
					getch();
			}
			useroption(arr);
			break;
		case 4:
			{
				float amount;
				cout <<"Enter Amount to deposit:";
				cin >>amount;
				if(amount > 0)
				{
					arr[0].balance=arr[0].balance+amount;
					cout <<"Amount Deposited Successfully! "<<endl;
				}
				else
				{
					cout<<"Invalid Input"<<endl;
				}
				
				ifstream infile;
				ofstream outfile;
				
				infile.open("employee.txt");
				outfile.open("temp.txt");
				
				if(!infile.is_open())
				{
					cout<<"Error in opening file for reading "<<endl;
					getch();
					return;
				}
				
				if(!outfile.is_open())
				{
					cout<<"Error: in opening file for writing"<<endl;
					getch();
					return;
				}
				
				addusers user;
				bool found= false;
				
				while(!infile.eof())
				{
					while(getline(infile, user.name))
					{
						getline(infile, user.fathername);
						getline(infile, user.cnic);
						getline(infile, user.phnumber);
						getline(infile, user.adress);
						infile >>user.pass;
						infile >>user.balance;
						infile.ignore();
						getline(infile, user.line);
						
						if(user.name != arr[0].name || user.pass != arr[0].pass)
						{
							outfile <<user.name<<endl;
							outfile <<user.fathername<<endl;
							outfile <<user.cnic<<endl;
							outfile <<user.phnumber<<endl;
							outfile <<user.adress<<endl;
							outfile <<user.pass<<endl;
							outfile <<user.balance<<endl;
							outfile <<user.line<<endl;
							found = false;
						}
						else if(user.name == arr[0].name && user.pass == arr[0].pass)
						{
							found=true;
						}
					}
				}
				infile.close();
				outfile.close();
					
				remove("employee.txt");
				rename("temp.txt", "employee.txt");
					
				ofstream obj;
				obj.open("employee.txt", ios::app);
				obj <<arr[0].name<<endl;
				obj <<arr[0].fathername<<endl;
				obj <<arr[0].cnic<<endl;
				obj <<arr[0].phnumber<<endl;
				obj <<arr[0].adress<<endl;
				obj <<arr[0].pass<<endl;
				obj <<arr[0].balance<<endl;
				obj <<arr[0].line<<endl;
				obj.close();
				cout<<"Press any key to continue"<<endl;
				getch();	
				
			}
			useroption(arr);			
			break;
		case 5:
			{
				float cash;
				cout <<"Enter Amount to Withdraw ";
				cin >>cash;
				if(cash > 0)
				{
					if(arr[0].balance > cash)
					{
						arr[0].balance=arr[0].balance-cash;
						cout <<"Amount Withdrawn Successfully! "<<endl;
					}
					else
					{
						cout<<"Insufficient Balance! "<<endl;	
					}
				}
				else
				{
					cout<<"Invalid Input"<<endl;
				}
				
				ifstream infile;
				ofstream outfile;
				infile.open("employee.txt");
				outfile.open("temp.txt");
				
				if(!infile.is_open())
				{
					cout<<"Error: could not open file for reading."<<endl;
					getch();
					return;
				}
				if(!outfile.is_open())
				{
					cout<<"Error: could not open file for writing"<<endl;
					getch();
					return;
				}
				
				addusers user;
				bool found=false;
				
				while(!infile.eof())
				{
					while(getline(infile, user.name))
					{
						getline(infile, user.fathername);
						getline(infile, user.cnic);
						getline(infile, user.phnumber);
						getline(infile, user.adress);
						infile >>user.pass;
						infile >>user.balance;
						infile.ignore();
						getline(infile, user.line);
					
						if(user.name != arr[0].name || user.pass != arr[0].pass)
						{
							outfile <<user.name<<endl;
							outfile <<user.fathername<<endl;
							outfile <<user.cnic<<endl;
							outfile <<user.phnumber<<endl;
							outfile <<user.adress<<endl;
							outfile <<user.pass<<endl;
							outfile <<user.balance<<endl;
							outfile <<user.line<<endl;
							found=false;			
						}
						else if(user.name == arr[0].name && user.pass == arr[0].pass)
						{
							found=true;
						}
					}
				}
				
				infile.close();
				outfile.close();
					
				remove("employee.txt");
				rename("temp.txt", "employee.txt");
					
				ofstream obj;
				obj.open("employee.txt", ios::app);
				obj <<arr[0].name<<endl;
				obj <<arr[0].fathername<<endl;
				obj <<arr[0].cnic<<endl;
				obj <<arr[0].phnumber<<endl;
				obj <<arr[0].adress<<endl;
				obj <<arr[0].pass<<endl;
				obj <<arr[0].balance<<endl;
				obj <<arr[0].line<<endl;
				obj.close();
				
			}
			cout <<"Press any key to continue"<<endl;
			getch();
			useroption(arr);
			break;
		case 6:
			cout<<"Exiting........"<<endl;
			break;
		default:
			cout<<"Invalid input...\nPress any key to continue"<<endl;
			getch();
			useroption(arr);
			
	}

}
//function for storing data in array
void storeinarray(string n,int p)
{
	system("cls");
	
	ifstream obj;
	obj.open("employee.txt");
	
	if(!obj.is_open())
	{
		cout<<"Error: could not open file for reading"<<endl;
		getch();
		return;
	}
	
	addusers user;
	addusers arr[1];
	
   	while(!obj.eof())
    {
   		while(getline(obj, user.name))
		{
			getline(obj, user.fathername);
			getline(obj, user.cnic);
			getline(obj, user.phnumber);
			getline(obj, user.adress);
			obj >> user.pass;
			obj >> user.balance;
			obj.ignore();
			getline(obj, user.line);
		
		
			//check if this match username and pass and then write it into array
			if(n == user.name && p == user.pass)
			{
				arr[0].name = user.name;
				arr[0].fathername = user.fathername;
				arr[0].cnic = user.cnic;
				arr[0].phnumber = user.phnumber;
				arr[0].adress = user.adress;
				arr[0].pass = user.pass;
				arr[0].balance = user.balance;
				arr[0].line = user.line;
			}
		}
    	obj.close();
    	useroption(arr);
	}
	
}

// function for user login
 void userlogin()
{
	system("cls");
	string name;
	int pass;
	cin.ignore();
	cout<<"Enter Your Name:"<<endl;
	getline(cin, name);
	cout<<"Enter Your Password:"<<endl;
	cin>>pass;
	
	ifstream obj("employee.txt");
	
	if(!obj.is_open())
	{
		cout<<"Error: could not open file for reading"<<endl;
		getch();
		return;
	}
	
	addusers user;
	bool found=false;
	

	while(getline(obj, user.name))
	{
		getline(obj, user.fathername);
		getline(obj, user.cnic);
		getline(obj, user.phnumber);
		getline(obj, user.adress);
		obj >> user.pass;
		obj >> user.balance;
		obj.ignore();
		getline(obj, user.line);
	
		if(name == user.name && pass == user.pass)
		{
			found = true;
			system("cls");
			cout<<"------------------"<<endl;
			cout<<"Access Guaranteed!"<<endl;
			cout<<"------------------"<<endl;
			getch();
			break; //exit the loop when user is found;
		}	
		
	}
	obj.close();
	if(found)
	{
		storeinarray(user.name,user.pass);
	}
	if(!found)
	{
		cout<<"Name: "<<name<<" with  pass (****) not found!"<<endl;
		getch();
		cout<<"Press any key to continue !";
		usermenu();
	}
	
}
//user menu
void usermenu()
{
	system("cls");
	int choice;
	cout<<"--------------------"<<endl;
	cout<<"Welcome To user menu"<<endl;
	cout<<"--------------------"<<endl;
	cout<<"1.Log-in"<<endl;
	cout<<"2.Sign-up"<<endl;
	cout<<"3.Back to Display menu"<<endl;
	cout<<"Enter your choice"<<endl;
	cin>>choice;
	switch(choice)
	{
		case 1:
			userlogin();
			break;
		case 2:  
			cout<<"Consult Manager(with CNIC) for account creation"<<endl;
			getch();
			displaymenu();
			break;
		case 3:
			displaymenu();
			break;
		default:
			cout<<"Invalid Choice....Press any key"<<endl;
			getch();
			usermenu();
			
	}
	
}

//dislay menu
void displaymenu(){
	
	system ("cls");
	cout << "---------------"<<endl;
	cout << " Welcome" << endl;
    cout << "---------------"<<endl;
    cout << "1.Manager Login" << endl;
    cout << "2.User Login" << endl;
    cout << "3.Exit"<<endl;
    cout << "---------------"<<endl;
    cout << "Enter your choice: ";
	int choice;
    
        
        cin >> choice; 
        switch (choice) 
		{
            case 1:
                managermenu();
                break;
            case 2:
            	usermenu();
            	break;
            case 3:
                cout << "Exiting the program." << endl;
                break;
            default:
                cout << "Invalid choice. Press any key to continue!" << endl;
                getch();
                displaymenu();
                
        	
    	}
}
