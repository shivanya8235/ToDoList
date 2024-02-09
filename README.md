# ToDoList project using cpp

#include<iostream>
#include<string>
#include<fstream>
using namespace std;

struct todolist
{
    int id;
    string task;
};

int ID;
void banner();
void addTask();
void viewTask();
int  markTask();
void removeTask();


int main(){
    system("cls");
    while(true)
    {
     banner();
     cout<<"\n\t.1 Add Task";
     cout<<"\n\t.2 View Task";
     cout<<"\n\t.3 Mark Task as completed";
     cout<<"\n\t.4 Remove Task";

     int choice;
     cout<<"\n\tEnter Choice: ";
     cin>>choice;
     switch(choice){
     case 1:
         addTask();
         break;
     case 2:
         viewTask();
         break;
     case 3:
         markTask();
         break;
     case 4:
         removeTask();
         break;
     default:
         break;
     }
    }
    return 0;
}

void banner(){
    cout<<"_______________________________"<<endl;
    cout<<"\t  My Todo List"<<endl;
    cout<<"_______________________________"<<endl;
}

void addTask(){
    system("cls");
    banner();
    todolist todo;
    cout<<"Enter new tsk: "<<endl;
    cin.get();
    getline(cin, todo.task);
    char save;
    cout<<"Save?(y/n):";
    cin>>save;
    if(save=='y'){
        ID++;
        ofstream fout;
        fout.open("todo.txt",ios::app);
        fout<<"\n"<<ID;
        fout<<"\n"<<todo.task;
        fout.close();

        char more;
        cout<<"Add more task?(y/n): ";
        cin>>more;

        if(more == 'y'){
            addTask();
        }
        else{
            system("cls");
            cout<<"Added Successfully!"<<endl;
            return;
        }
    }
    system("cls");
}

void viewTask(){
    system("cls");
    banner();
    todolist todo;
    ifstream fin;
    fin.open("todo.txt");
    cout<<"Task: "<<endl;

    while(!fin.eof()){
        fin>>todo.id;
        fin.ignore();
        getline(fin,todo.task);
        if(todo.task!=""){
            cout<<"\t"<<todo.id<<": "<<todo.task<<endl;
        }
        else{
            cout<<"\tEmpty!"<<endl;
        }
    }
    fin.close();
    char exit;
    cout<<"Exit(y/n): ";
    cin>>exit;
    if(exit!= 'y'){
        viewTask();
    }
    system("cls");
}

int markTask(){
    system("cls");
    banner();
    int id;
    cout<<"Enter task id: ";
    cin>>id;
    todolist todo;
    ifstream fin("todo.txt");
    while(!fin.eof()){
        fin>>todo.id;
        fin.ignore();
        getline(fin,todo.task);
        if(todo.id == id){
            system("cls");
            cout<<"\t"<<todo.id<<": "<<todo.task<<"->marked"<<endl;
            return id;
        }
    }
    system("cls");
    cout<<"Not marked!"<<endl;
    return 0;
}

void removeTask(){
    system("cls");
    int id=markTask();
    if(id !=0){
       char del;
       cout<<"\n\tRemove?(y/n): ";
       cin>>del;
       if(del=='y'){
        todolist todo;
        ofstream tempFile;
        tempFile.open("temp.txt");
        ifstream fin;
        fin.open("todo.txt");
        int index =1;
        while(fin.eof()){
            fin>>todo.id;
            fin.ignore();
            getline(fin,todo.task);
            if(todo.id !=id){
                tempFile<<"\n"<<index;
                tempFile<<"\n"<<todo.task;
                index++;
                ID--;
            }
        }
        fin.close();
        tempFile.close();
        remove("todo.txt");
        rename("temp.txt","todo.txt");
        system("cls");
        cout<<"\n\tRemoved Successfully!"<<endl;
       }
       else{
        system("cls");
        cout<<"Not Removed!"<<endl;
       }
    }
}
