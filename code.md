#include<iostream> 
#include<string>
#include<fstream>
#include<iterator>
using namespace std;

class Crypto{
public:
	string Xor(const string& text,int key){
		string result;
		for(int i=0;i<text.size();i++){
		char ch=text[i];
 		result+=ch^key;
}
		return result;
	}
};
class FileHandler{
public:
	string read(const string& filepath){
		ifstream file(filepath.c_str());
		string content((istreambuf_iterator<char>(file)),{});
		file.close();
		return content;
	}
	void write(const string& filepath,const string& content){
		ofstream file(filepath.c_str());
		file<<content;
		file.close();
		cout<<"saved successfully";
	}
};
class Menu{
private:
	Crypto target;
	FileHandler fileact;
public:
	void handletext(){
		string text,result;
		int key;
		cout<<"please input the text:"<<endl;
		getline(cin,text);
		cout<<"please input the key:"<<endl;
		cin>>key;
		result=target.Xor(text,key);
		cout<<"The result is:"<<result<<endl;
	}
	void handlefile(){
		string in,out,text,result;
		int key;
		cout<<"input the infilepath"<<endl;
		getline(cin,in);
		text=fileact.read(in);
		cout<<"input the key"<<endl;
		cin>>key;
		result=target.Xor(text,key);
		cout<<"input the outfilepath"<<endl;
		getline(cin,out);
		fileact.write(out,result);
	}
	void showMenu(){
		int choice;
		cout<<"请输入加密选项："<<endl;
		cout<<"文本加密/解密——1"<<endl;
		cout<<"文件加密/解密——2"<<endl;
		cin>>choice;
		cin.ignore();
		switch(choice){
			case 1:
				handletext();
				break;
			case 2:
				handlefile();
				break;
			default:
				cout<<"Wrong input!"<<endl;
		} 
	}
};
int main(){
	Menu start;
	start.showMenu();
	return 0;
}
