//TIME CODE ADDITIONS  is Class node , insert_class, insert_emptyclass, count_to_time . time_to count

#include <iostream>
#include <fstream>
#include <windows.h>
#include <conio.h>
#include<string.h>
#include<cstring>

using namespace std;


//------------------------ MISCELLANEOUS FUNcTION-----------------------------------
void setColor(unsigned short color) {

	SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE), color);
}

char* GetStringFromBuffer(char* buffer)
{
	int strLen = strlen(buffer);
	char* str = new char[strLen + 1];//null hojata 
	if (strLen > 0) {
		str = new char[strLen + 1];	//This function should allocate required memory on heap, 
		char* tempDest = str;

		for (char* tempSrc = buffer; *tempSrc != '\0'; tempSrc++, tempDest++) {
			*tempDest = *tempSrc;//copy string in this memory and
		}
		*tempDest = '\0';	// DO NOT FORGET TO KEEP SPACE FOR NULL CHARACTER AND MARKING THE END OF STRING
	}
	return str;// return newly created cstring.
}

char* GetStringFromBuffer(const char* buffer)
{
	int strLen = strlen(buffer);
	char* str = new char[strLen + 1];//null hojata 
	if (strLen > 0) {
		str = new char[strLen + 1];	//This function should allocate required memory on heap, 
		char* tempDest = str;

		for (const char* tempSrc = buffer; *tempSrc != '\0'; tempSrc++, tempDest++) {
			*tempDest = *tempSrc;//copy string in this memory and
		}
		*tempDest = '\0';	// DO NOT FORGET TO KEEP SPACE FOR NULL CHARACTER AND MARKING THE END OF STRING
	}
	return str;// return newly created cstring.
}

// Returns true if s1 is substring of s2   //SOURCE: https://www.geeksforgeeks.org/check-string-substring-another/
int isSubstring(const char* s1, const  char* s2)
{
	int M = strlen(s1);
	int N = strlen(s2);

	if (M == 2)
		return -1; //Avoids the danger k CS ko vo CSA1&CSA2 MEIN DHOODH LEY :)


				   /* A loop to slide pat[] one by one */
	for (int i = 0; i <= N - M; i++)
	{
		int j;

		/* For current index i, check for pattern match */
		for (j = 0; j < M; j++)
			if (s2[i + j] != s1[j])
				break;

		if (j == M)
			return i;  //EVEN GIVES THE INDEX AT WHICH THE SUBSTRING IS .... WE DON'T DESERVE FUNCTIONS LIKE THESE 
	}

	return -1; //false
}

char * return_Name_and_Section(char coursename[], char*& n) //friend of time table adt of this was OOP
{
	//const char* coursename = "SAI (CS-Fss99)";//<--- non-empty with guarenteed '('
	int i = 0;

	bool is_ReasearchMeeting = true;
	for (int i = 0; i < strlen(coursename); i++)
	{
		if (coursename[i] == '(')
			is_ReasearchMeeting = false;
	}

	if (is_ReasearchMeeting == true)
	{
		n = GetStringFromBuffer(coursename);
		char* empty = new char[2];
		empty[0] = ' ';
		empty[1] = '\0';
		return empty;
	}
	for (; coursename[i] != '('; i++)  //GET LENGHT till '('
	{
		;
	}
	char * name = new char[i - 1]; //SPACE B4 ' '( PER '\0' GHUSAAO
								   /*cout << i;*/
	int y = 0;
	for (; y < i; y++)//COPY SH*T
	{
		name[y] = coursename[y];
	}

	name[y - 1] = '\0'; // END IT

	n = GetStringFromBuffer(name);

	//cout << name << endl;  //<-----------------------NAAM OF COURSE

	//cout<< coursename[i]; // has '('

	int i_ = 0;
	for (; coursename[i_ + i + 1] != ')'; i_++)  //GET LENGHT
	{
	}
	//i_--;

	char * name2 = new char[i_ + 1]; //')'  tak

	int y_ = 0;

	for (; y_ < i_ + 1; y_++)//COPY SHIT
	{
		name2[y_] = coursename[y_ + i + 1];
	}

	name2[i_] = '\0'; // END IT

					  //cout << name2 << endl;  //<-----------------------NAAM OF SECTION

	return name2;
}

//helper class
class hlp {
public:
	char * roll;
	char * name;
	char* course;
	char* section;
	char* room;
	char * st_time;
	char* end_time;
};
//--------------------------END OF MISCELLANEOUS FUNCTIONA-------------------------





//------------------------ START OF CLASSLL----------------------------------------
struct classlist
{
	int count;
	char * names[200]; //60 students in class, first name only.
	char * rollNo[200];

	classlist()
	{
		count = 0;
		for (int i = 0; i < 60; i++)
		{
			names[i] = 0;
			rollNo[i] = 0;
		}
		//	void find roll number()
	}

	void printClassList() //non-part-C function
	{
		for (int c = 0; c < count; c++)
		{
			cout << names[c] << " " << rollNo[c] << endl;
		}
	}

	bool RollNumberFound(const char* rollno) //rt-C function
	{
		for (int c = 0; c < count; c++)
		{
			if (strcmp(rollNo[c], GetStringFromBuffer(rollno)) == 0)
				return true;
		}
		return false;
	}

};
class ClassNode
{
public:
	char *course_name;
	char *section;


	int count; //............not used but still being populated..........TIME CODE ADDITION The count inside ClassLL  (used in timings)
	char* start_time;
	char* end_time;

	classlist* ptr; //ThisClass;
	ClassNode* next;
	ClassNode()
	{
		ptr = 0;
		next = 0;
	}

};
class ClassLL
{
public:
	ClassNode* first;

	ClassLL() {
		first = 0;
	}

	void insert_class(char* Name, char* Section, int j)  //insert the node at the end.
	{
		ClassNode* temp;
		temp = new  ClassNode;

		temp->ptr = 0;
		temp->next = 0;
		temp->course_name = GetStringFromBuffer(Name);
		temp->section = GetStringFromBuffer(Section);
		temp->count = 0;
		if (j == 0) {
			temp->start_time = GetStringFromBuffer("8:00");
			temp->end_time = GetStringFromBuffer("9:30");
		}
		else if (j == 1)
		{
			temp->start_time = GetStringFromBuffer("9:30");
			temp->end_time = GetStringFromBuffer("11:00");
		}
		else if (j == 2)
		{
			temp->start_time = GetStringFromBuffer("11:00");
			temp->end_time = GetStringFromBuffer("12:30");
		}
		else if (j == 3)
		{
			temp->start_time = GetStringFromBuffer("12:30");
			temp->end_time = GetStringFromBuffer("2:00");
		}
		else if (j == 4)
		{
			temp->start_time = GetStringFromBuffer("2:00");
			temp->end_time = GetStringFromBuffer("3:30");
		}
		else if (j == 5)
		{
			temp->start_time = GetStringFromBuffer("3:30");
			temp->end_time = GetStringFromBuffer("5:00");
		}
		else if (j == 6)
		{
			temp->start_time = GetStringFromBuffer("5:00");
			temp->end_time = GetStringFromBuffer("6:30");
		}
		else if (j == 7)
		{
			temp->start_time = GetStringFromBuffer("6:30");
			temp->end_time = GetStringFromBuffer("8:00");
		}


		if (!first) //first==0
		{
			first = temp;

		}
		else {
			temp->count = 0;

			ClassNode* curr = first;

			while (curr->next != 0)
			{

				curr = curr->next;

				temp->count = temp->count + 1;//...................................................................................time code addition
			}
			temp->count = temp->count + 1;
			curr->next = temp;
		}
	}

	void insert_emptyclass(char*name, int j) {
		ClassNode* temp;
		temp = new  ClassNode;

		temp->ptr = 0;
		temp->next = 0;
		temp->course_name = GetStringFromBuffer(" ");
		temp->section = GetStringFromBuffer(" ");
		temp->count = 0;


		temp->start_time = GetStringFromBuffer(" ");
		temp->end_time = GetStringFromBuffer(" ");

		if (!first) {
			first = temp;
		}
		else {

			ClassNode* curr = first;
			while (curr->next != 0) {
				curr = curr->next;
				temp->count += 1;
			}
			temp->count += 1;
			curr->next = temp;
		}

	}

	~ClassLL()
	{
		ClassNode* curr = first;
		while (curr != 0) {
			first = first->next;
			delete curr;
			curr = first;
		}
	}


	//####### Phase B's star function #########
	int TraverseClassList(char* CourseName, char* Section, classlist  * ptr_)
	{
		ClassNode* curr = first;
		int countt = 0;

		while (curr != 0)
		{
			if (strcmp(curr->course_name, CourseName) == 0 && (strcmp(curr->section, Section) == 0 || isSubstring(Section, curr->section) != -1))
			{
				//if (curr->ptr != 0)// A1 HAS BEEN POPULATED AND A2_PTR HAS TO BE POPULATED :(
				//{
				//	cout << " Ovverriding  occured for" << CourseName << " " << Section << endl;
				//}

				//	//int ariana = curr->count;
				//	//cout << "ariana: " << ariana << "ptr_ ka count::";
				//	//cout << ptr_->names[0]; //<< masla
				//	//_getch();
				//	//ptr_->count--; //curr points to empty thing 
				//	//while (ptr_->count >= 0)
				//	//{
				//	//	curr->ptr->names[ariana] = ptr_->names[ptr_->count];
				//	//	curr->ptr->rollNo[ariana] = ptr_->rollNo[ptr_->count];
				//	//	curr->count++;
				//	//	ariana++;
				//	//	ptr_->count--;
				//	//}
				//	//countt++;
				//}
				//abhi case ni handle hua k baaqi naam walay bas point kerain


				if (curr->ptr == 0) //THIS WILL HAPPEN FIRST
				{
					//cout << "-----------------" << ptr_->count; _getch();
					curr->ptr = ptr_; //attachment

					curr->count = ptr_->count; //this is needed when ovverwriting is handeled :)
											   //!!!!!!!!!!!!!!!!!!WHILE CURR PTR_ WAS BEING POULATED it's COUNT WAS UPDATED :)

											   //ptr_ = 0; //null kerdijiye takay ptr_ ki shahdat k waqt ptr na urr jaye :)  //null hosakta
					countt++;
				}
			}
			curr = curr->next;
		}

		return countt; //is 2 if it was a lab :)
	}

	void PrintClassNames() {
		ClassNode*curr = first;
		while (curr != 0) {
			cout << "----|  " << curr->course_name << "	" << curr->section << "  " << curr->start_time << curr->end_time << endl;
			curr = curr->next;
		}
	}

	//==============PHASE B==============================
	const char* count_to_time(int count_)
	{
		switch (count_) {

		case 0: 	return "8:00 to 9:30";           break;
		case 1:     return "9:30 to 11:00";          break;
		case 2:     return "11:00 to 12:30";         break;
		case 3:     return "12:30 to 2:00";          break;
		case 4:     return "2:00 to 3:30";           break;
		case 5:     return "3:30 to 5:00";           break;
		case 6:     return "5:00 to 6:30";           break;
		case 7:     return "6:30 to 8:00";           break;
		default:    return "101 error occured :(";   break;
		}
	}

	//parameter (8:00) or (8:00 to 9:30)
	int time_to_count(const char * c_) //starting time && starting+ending donnon k liye kaam :)
	{
		if (strcmp(c_, "8:00") == 0 || strcmp(c_, "8:00 to 9:30") == 0)
			return 0;
		else if (strcmp(c_, "9:30") == 0 || strcmp(c_, "9:30 to 11:00") == 0)
			return 1;
		else if (strcmp(c_, "11:00") == 0 || strcmp(c_, "11:00 to 12:30") == 0)
			return 2;
		else if (strcmp(c_, "12:30") == 0 || strcmp(c_, "12:30 to 2:00") == 0)
			return 3;
		else if (strcmp(c_, "2:00") == 0 || strcmp(c_, "2:00 to 3:30") == 0)
			return 4;
		else if (strcmp(c_, "3:30") == 0 || strcmp(c_, "3:30 to 5:00") == 0)
			return 5;
		else if (strcmp(c_, "5:00") == 0 || strcmp(c_, "5:00 to 6:30") == 0)
			return 6;
		else if (strcmp(c_, "6:30") == 0 || strcmp(c_, "6:30 to 8:00") == 0)
			return 7;
	}

	//---------------------C(2)----------------------------
	void Find_5_(char * coursename, char *section, char* dayname)
	{
		ClassNode*curr = first;
		while (curr != 0)
		{
			if (strcmp(curr->course_name, coursename) == 0 && strcmp(curr->section, section) == 0) //print the class timing of a particular course
			{
				cout << dayname << " "; cout << curr->start_time << "," << curr->end_time << "||" << endl;
			}
			curr = curr->next;
		}
	}

	void Find_2(const char * rollno, char* day)
	{

		//   /*<course 1 with section>:
		//   <day> -<starttime> : <endtime>, <classroom> || <day> -<starttime> : <endtime>, <classroom>, …
		//   <day> -<starttime> : <endtime>, <classroom> || <day> -<starttime> : <endtime>, <classroom>, …

		//<course 2 with section> :
		//<day> -<starttime> : <endtime>, <classroom> || <day> -<starttime> : <endtime>, <classroom>, …
		//<day> -<starttime> : <endtime>, <classroom> || <day> -<starttime> : <endtime>, <classroom>, …

		ClassNode*curr = first;

		while (curr != 0)
		{

			if (curr->ptr != 0) {
				if (curr->ptr->RollNumberFound(rollno) == true)
				{
					cout << curr->course_name << " (" << curr->section << "):\n";
					cout << day << curr->start_time << " " << curr->end_time;
					//Programming Fundamentals (CS-B): 
					//Wednesday 9:30 : 10 : 50, CS - 1 || Friday 9 : 30 : 10 : 50, CS - 5
				}
			}
			curr = curr->next;
		}
	}

	//---------------------C(3)----------------------------
	void Find_3(const char * rollno)
	{
		ClassNode*curr = first;

		while (curr != 0)
		{

			if (curr->ptr != 0)
			{
				if (curr->ptr->RollNumberFound(rollno) == true)
				{
					cout << curr->course_name << " " << curr->section << endl;
				}

			}
			curr = curr->next;
		}
	}

	//---------------------C(4)----------------------------
	void Find_4(const char *time) ////Print the course name with section
	{
		ClassNode*curr = first;    int x = time_to_count(time);

		for (int i = 0; i < x; i++) {
			curr = curr->next;
		}
		cout << curr->course_name << "\t" << curr->section << endl;
	};

	//--------------------C(5)------------------------------
	void Find_5(const char * coursename, const char *section, char* dayname)
	{
		ClassNode*curr = first;
		while (curr != 0)
		{
			if (strcmp(curr->course_name, coursename) == 0 && strcmp(curr->section, section) == 0) //print the class timing of a particular course
			{
				cout << dayname << " ";
				cout << curr->start_time << endl;
			}
			curr = curr->next;
		}
	}

	char* find_and_set(char* course, char*section) {
		ClassNode* curr = first;
		char* time = 0;

		while (curr != 0) {
			if (strcmp(curr->course_name, course) == 0 && strcmp(curr->section, section) == 0)
			{
				time = curr->start_time;
				break;
			}
			curr = curr->next;
		}

		return time;
	}

	char* find_and_set2(char* course, char*section) {
		ClassNode* curr = first;
		char* time = 0;
		char* lab = { "Lab" };
		int isLab = -1;
		isLab = isSubstring(lab, course);
		while (curr != 0) {
			if (isLab != -1) {
				if (strcmp(curr->course_name, course) == 0 && strcmp(curr->section, section) == 0)
				{
					time = curr->next->end_time;
					break;
				}
			}
			else {
				if (strcmp(curr->course_name, course) == 0 && strcmp(curr->section, section) == 0)
				{
					time = curr->end_time;
					break;
				}
			}

			curr = curr->next;
		}
		return time;

	}

	bool inroom(char*course, char*section) {
		ClassNode* curr = first;
		bool isfound = false;
		while (curr != 0) {
			if (strcmp(curr->course_name, course) == 0 && strcmp(curr->section, section) == 0)
			{
				isfound = true;

			}
			curr = curr->next;
		}
		return isfound;
	}

	bool inTT(char* course, char*section) {
		ClassNode* curr = first;
		bool isfound = false;
		while (curr != 0) {
			if (strcmp(curr->course_name, course) == 0 && strcmp(curr->section, section) == 0)
			{
				isfound = true;

			}
			curr = curr->next;
		}
		return isfound;
	}

};
//-------------------------END OF CLASSLL------------------------------------------





//------------------------ START OF ROOMLL-----------------------------------------
class RoomNode
{
public:
	ClassLL  ClassList;
	RoomNode * next;
	char * roomName; //

	RoomNode()
	{
		next = 0;
		roomName = NULL;
	}
};
class RoomLL
{
public:
	RoomNode * first;
	RoomNode * current;

	RoomLL() //CONSTRUCTOR
	{
		first = 0;
	}

	~RoomLL() //DESTRUCTOR
	{
		RoomNode* curr = first;
		while (curr != 0) {
			first = first->next;
			delete curr;
			curr = first;
		}
	}

	void PrintRoomNames()
	{
		RoomNode * curr = first;
		while (curr != 0)
		{
			cout << curr->roomName << endl;
			curr->ClassList.PrintClassNames();
			curr = curr->next;
		}
	}


	//*POPULATION FUNCTIONS*
	void insert_room_name(char*Name)  //insert the node at the end.
	{
		RoomNode * temp;
		temp = new  RoomNode;
		temp->next = 0;
		temp->roomName = GetStringFromBuffer(Name);
		//temp->ClassList  if  populated by insert_class_name() during reading

		if (!first) {
			first = temp;
			current = first;
		}
		else
		{
			RoomNode* curr = first;

			while (curr->next != 0) {
				curr = curr->next;
			}
			curr->next = temp;
			current = curr->next;
		}
	}

	void insert_class_name(char*name, char*section, int j)
	{
		current->ClassList.insert_class(name, section, j);

	}

	void insert_class_name(char*name, int j)
	{
		current->ClassList.insert_emptyclass(name, j);
	}


	//*SEARCHING  FUNCTIONS*
	void Traverse_Room_List(char* CourseName, char* Section, classlist  * ptr_, int &timesithappened)
	{
		RoomNode * curr = first;
		while (curr != 0)
		{
			timesithappened = curr->ClassList.TraverseClassList(CourseName, Section, ptr_);
			curr = curr->next;
		}
	}

	void Find5_(const char * coursename, const char *section, char* dayName)
	{
		RoomNode * curr = first;

		while (curr != 0)
		{

			curr->ClassList.Find_5(coursename, section, dayName); //Print the course name with section
			curr = curr->next;

		}
	}

	void Find2(const char *RollNumber, char* day)
	{
		RoomNode * curr = first;
		while (curr != 0)
		{
			curr->ClassList.Find_2(RollNumber, day); //Print the course name with section	

			curr = curr->next;
		}

	}

	//-----------------------C(3)-------------------------
	void Find3(const char *RollNumber)
	{
		RoomNode * curr = first;
		while (curr != 0)
		{
			curr->ClassList.Find_3(RollNumber); //Print the course name with section	

			curr = curr->next;
		}
	}

	//-----------------------C(5)-------------------------
	void Find4(const char *time, const char *classroom)
	{
		RoomNode * curr = first;
		while (curr != 0)
		{
			if (strcmp(curr->roomName, GetStringFromBuffer(classroom)) == 0)
			{
				curr->ClassList.Find_4(time); //Print the course name with section
				break;
			}
			curr = curr->next;
		}
	}

	//----------------------C(5)--------------------
	void Find5(const char * coursename, const char *section, char* dayName)
	{
		RoomNode * curr = first;

		while (curr != 0)
		{

			curr->ClassList.Find_5(coursename, section, dayName); //Print the course name with section
			curr = curr->next;

		}
	}

	char* search_and_set(char* course, char*section) {
		RoomNode* curr = first;
		char*time = 0;
		while (curr != 0) {
			time = curr->ClassList.find_and_set(course, section);
			if (time != NULL) {
				break;
			}
			curr = curr->next;
		}


		return time;
	}

	char* search_and_set2(char*course, char*section) {
		RoomNode* curr = first;
		char*time = 0;
		while (curr != 0) {
			time = curr->ClassList.find_and_set2(course, section);
			if (time != NULL) {
				break;
			}
			curr = curr->next;
		}
		return time;
	}

	char* GetRoomName(char*course, char*section) {
		RoomNode*curr = first;
		char*name = 0;
		bool check = false;
		while (curr != 0) {
			check = curr->ClassList.inroom(course, section);
			if (check == true) {
				name = curr->roomName;
			}
			curr = curr->next;
		}
		return name;
	}

	bool inTTADT(char*course, char* section) {
		RoomNode*curr = first;
		char*name = 0;
		bool check = false;
		while (curr != 0) {
			check = curr->ClassList.inroom(course, section);
			if (check == true) {
				return true;
			}
			curr = curr->next;
		}
		return check;
	}


};
//-------------------------END OF ROOMLL-------------------------------------------





//------------------------ START OF DAY--------------------------------------------
class Day
{
public:
	RoomLL Roomlist;
	char *name;
	Day()
	{
		name = 0;
	}


};
//------------------------ END OF DAY--------------------------------------------



//--------- START OF A CLASS USED-IN-PHASE-B <3 {not all heroes wear capes}--------
class regdataLine //acts as node
{
public:
	int rollno;//Used in PHASE-C

	char* Name;

	char* aftername;   //Section and Course compared in PhaseB
	bool has_been_used;

	void Printline()
	{
		cout << rollno << "," << Name << "," << aftername << "\n";
	}

};

class regDataADT
{
public:
	regdataLine Object[8003];

	void fileToADT(const char filename[])
	{
		ifstream fin;  fin.open(filename);

		char junk[499]; int x;
		fin.getline(junk, 439, '\n');

		int i = 0;
		while (!fin.eof()) {

			fin >> x;          Object[i].rollno = x;

			fin.ignore(1); //Skips "," in file
			fin.getline(junk, 200, ',');          Object[i].Name = GetStringFromBuffer(junk);

			fin.getline(junk, 300, '\n');        Object[i].aftername = GetStringFromBuffer(junk);
			//cout << "ME!";

			Object[i].has_been_used = false;
			i++;
		}
		cout << i;
		/*for (int i_ = 0; i_ < i; i_++)
		Object[i_].Printline();*/
	}
	void SortTheADT()
	{
		ofstream fout; fout.open("phaseb.csv");             setColor(14);

		int times_PRINTED = 0;

		for (int i = 0; i < 8003; i++)
		{
			if (Object[i].has_been_used == false)
			{
				Object[i].has_been_used = true;
				fout << Object[i].rollno << "," << Object[i].Name << "," << Object[i].aftername << "\n";

				//Object[i].Printline(); 
				times_PRINTED++;

				for (int j = i + 1; j < 8003 /*&& *//*j < i + 1000*/; j++)
				{
					if (strcmp(Object[i].aftername, Object[j].aftername) == 0)
					{
						if (Object[j].has_been_used == false)
						{
							//Object[j].Printline();
							times_PRINTED++;
							fout << Object[j].rollno << "," << Object[j].Name << "," << Object[j].aftername << "\n";
							Object[j].has_been_used = true;
						}
					}
				}

			}
		}
		cout << times_PRINTED;
		//_getch();

		//----------------------PHASE C (NEW FILE NAMED PHASEC.CSV -----------------------------O.O
		fout.close();  fout.open("phasec.csv");

		for (int i = 0; i < 8003; i++)
			Object[i].has_been_used = false;

		//==
		for (int i = 0; i < 8003; i++)
		{
			if (Object[i].has_been_used == false)
			{
				Object[i].has_been_used = true;
				fout << Object[i].rollno << "," << Object[i].Name << "," << Object[i].aftername << "\n";

				//Object[i].Printline(); 
				//times_PRINTED++;

				for (int j = i + 1; j < 8003; j++)
				{
					if (Object[i].rollno == Object[j].rollno)
					{
						if (Object[j].has_been_used == false)
						{
							//Object[j].Printline();
							//times_PRINTED++;
							fout << Object[j].rollno << "," << Object[j].Name << "," << Object[j].aftername << "\n";
							Object[j].has_been_used = true;
						}
					}
				}

			}
		}
		fout.close();


	}
};

//--------- END OF A CLASS USED-IN-PHASE-B <3 {not all heroes wear capes}--------




//--------- START OF A CLASS USED-IN-PHASE-c <3 {not all heroes wear capes}------

//-------------------------END OF PhaseB functions-------------------------------------------


//------------------------ START OF timeTableADT --------------------------------------------
class TimeTable
{
private:
	Day Monday, Tuesday, Wednesday, Thursday, Friday;


	//-----------INTERNAL FUNCTIONS OF LOAD() {PHASE A}
	void loadMonday(ifstream    &fin)// Ideally reuse code for all days
	{
		fin.ignore(798); char x[65]; char Room_Name[65];

		for (int i = 0; i < 26; i++)
		{
			fin.getline(Room_Name, 50, ',');
			//setColor(14);		cout << Room_Name << endl;  setColor(12); //<<<<<<<<<<<<<<<<
			Monday.Roomlist.insert_room_name(Room_Name);

			for (int j = 0; j < 8; j++) {

				fin.getline(x, 65, ',');   //cout << x << endl; 

				if (strlen(x) > 5) {

					char* name1; char* section = GetStringFromBuffer(return_Name_and_Section(x, name1));

					//	setColor(15);	cout << name1 << "\t\t"; setColor(11); cout << section << endl; setColor(12);//<<<<<<<<<<<<

					Monday.Roomlist.insert_class_name(name1, section, j);
				}
				else
				{
					//	cout << x << endl;
					Monday.Roomlist.insert_class_name(x, j);
				}
				fin.ignore(8);
			}
			fin.ignore(23);
		}
		//system("cls");
		//Monday.Roomlist.PrintRoomNames(); 
		//_getch();

	}

	void loadTuesday(ifstream   & fin)
	{
		system("cls");
		fin.ignore(104);

		char x[95]; char Room_Name[65];
		for (int i = 0; i < 24; i++)
		{
			fin.getline(Room_Name, 50, ',');
			//setColor(14);		cout << Room_Name << endl; setColor(12); //<<<<<<<<<<<<<<
			Tuesday.Roomlist.insert_room_name(Room_Name);
			for (int j = 0; j < 8; j++)
			{
				fin.getline(x, 65, ',');
				//cout << x << endl;

				if (strlen(x) > 5)
				{
					char* name1; char* section = GetStringFromBuffer(return_Name_and_Section(x, name1));

					//setColor(15); cout << name1 << "\t\t"; setColor(11); cout << section << endl;//<<<<<<<<<<<<<<<<<
					//setColor(12);

					Tuesday.Roomlist.insert_class_name(name1, section, j);
				}
				else
				{
					//cout << x << endl; //<<<<<<<<<<<<<<<<<
					Tuesday.Roomlist.insert_class_name(x, j);
				}
				fin.ignore(8);
			}
			/*cout << "---------------------------------\n";*/
			fin.ignore(23);

		}
		//system("cls");
		//	Tuesday.Roomlist.PrintRoomNames();

	}

	void loadWednesday(ifstream & fin)
	{
		system("cls");
		fin.ignore(105);

		char x[95]; char Room_Name[65];
		for (int i = 0; i < 26; i++)
		{
			fin.getline(Room_Name, 50, ',');
			//setColor(14); cout << Room_Name << endl; setColor(12);//<<<<<<<<<<<<<<<<<<<<<<<<<<
			Wednesday.Roomlist.insert_room_name(Room_Name);
			for (int j = 0; j < 8; j++)
			{
				fin.getline(x, 65, ',');
				//cout << x << endl;
				if (strlen(x) > 5)
				{

					char* name1; char* section = GetStringFromBuffer(return_Name_and_Section(x, name1));

					//setColor(15);	cout << name1 << "\t\t"; setColor(11); cout << section << endl; setColor(12);//<<<<<<<<<

					Wednesday.Roomlist.insert_class_name(name1, section, j);
				}
				else
				{

					//cout << x << endl;//<<<<<<<<<
					Wednesday.Roomlist.insert_class_name(x, j);
				}
				fin.ignore(8);

			}
			fin.ignore(23);
		}
		//system("cls");
		//	Wednesday.Roomlist.PrintRoomNames();
	}

	void loadThursday(ifstream  & fin)
	{
		//system("cls");
		fin.ignore(104);
		char x[95]; char Room_Name[65];
		for (int i = 0; i < 25; i++)
		{
			fin.getline(Room_Name, 50, ',');
			//setColor(14); cout << Room_Name << endl; setColor(12);//<<<<<<<<<<<<<<<<<
			Thursday.Roomlist.insert_room_name(Room_Name);
			for (int i = 0; i < 8; i++)
			{
				fin.getline(x, 65, ',');
				//cout << x << endl;
				if (strlen(x) > 5)
				{
					char* name1;
					char*section = GetStringFromBuffer(return_Name_and_Section(x, name1));

					//setColor(15); cout << name1 << "\t\t"; setColor(11); cout << section << endl; setColor(12);//<<<<<<<<<
					Thursday.Roomlist.insert_class_name(name1, section, i);
				}
				else {
					//cout << x << endl; //<<<<<<<<<<<<
					Thursday.Roomlist.insert_class_name(x, i);
				}
				fin.ignore(8);
			}
			fin.ignore(23);
		}
		//system("cls");
		//	Thursday.Roomlist.PrintRoomNames();
	}

	void loadFriday(ifstream    & fin)
	{
		//system("cls");
		fin.ignore(102);
		char x[95]; char Room_Name[65];
		for (int i = 0; i < 26; i++) {
			fin.getline(Room_Name, 50, ',');

			//	setColor(14); cout << Room_Name << endl; setColor(12); //<<<<<<<<<<<<<<<<<<<<<<

			Friday.Roomlist.insert_room_name(Room_Name);
			for (int i = 0; i < 8; i++)
			{
				fin.getline(x, 65, ',');

				if (strlen(x) > 5) {
					char* name1;
					char*section = GetStringFromBuffer(return_Name_and_Section(x, name1));

					//setColor(15); cout << name1 << "\t\t"; setColor(11); cout << section << endl; setColor(12);//<<<<<<

					Friday.Roomlist.insert_class_name(name1, section, i);
				}
				else {
					//	cout << x << endl; //<<<<<<<<<<<<<<<
					Friday.Roomlist.insert_class_name(x, i);
				}
				fin.ignore(8);
			}
			fin.ignore(23);
		}
		//system("cls");
		//Friday.Roomlist.PrintRoomNames();
	}

public:
	//--------------PHASE A-----------------------
	void Load(const char filename[])
	{
		Monday.name = GetStringFromBuffer("Monday");
		Tuesday.name = GetStringFromBuffer("Tuesday");
		Wednesday.name = GetStringFromBuffer("Wednesday");
		Thursday.name = GetStringFromBuffer("Thursday");
		Friday.name = GetStringFromBuffer("Friday");


		ifstream fin;   fin.open(filename);

		if (fin.is_open()) {

			cout << filename << " Opened \n"; //setColor(8);

			loadMonday(fin);
			loadTuesday(fin);
			loadWednesday(fin);
			loadThursday(fin);
			loadFriday(fin);

			//system("cls"); 
			setColor(11);
			cout << "\nLOADING ACCOMPLISHED\n";
		}
		else
			cout << "DIDNOT OPEN -_-";
	}

	//------------------PHASE B------------------------------------------------------------------------------------------
	~TimeTable()
	{
		//Nothing is dynamic is ADT <3
		// Say Mashallah :)
		cout << "~TimeTable() called :) :) :)";
	}

	void Search_in_TTADT_plus_do_work_(char* CourseName, char* Section, classlist  * ptr_)
	{
		//ptr_->printClassList();

		//cout <<" all eyes on me"<< " "<< ptr_->count; _getch();

		int x = 0;//times_the_michael_angelo_moment_happened

		Monday.Roomlist.Traverse_Room_List(CourseName, Section, ptr_, x);
		Tuesday.Roomlist.Traverse_Room_List(CourseName, Section, ptr_, x);
		Wednesday.Roomlist.Traverse_Room_List(CourseName, Section, ptr_, x);
		Thursday.Roomlist.Traverse_Room_List(CourseName, Section, ptr_, x);
		Friday.Roomlist.Traverse_Room_List(CourseName, Section, ptr_, x);
		//if (x == 2) return;
		//if (x == 1) {

		//	Wednesday.Roomlist.Traverse_Room_List(CourseName, Section, ptr_, x);

		//	if (x == 1)
		//	{
		//		Friday.Roomlist.Traverse_Room_List(CourseName, Section, ptr_, x);
		//	}
		//}
		//if (x == 0)
		//{
		//	Tuesday.Roomlist.Traverse_Room_List(CourseName, Section, ptr_, x);
		//	if (x == 2) return;
		//	if (x == 1) {
		//		Thursday.Roomlist.Traverse_Room_List(CourseName, Section, ptr_, x);
		//	}
		//}
		//if (x == 0)
		//	Friday.Roomlist.Traverse_Room_List(CourseName, Section, ptr_, x);

		ptr_ = 0; //krna chahiye

	}

	void loadStudentInfo(const char filename[])
	{

		//TIME CONSMING 3 LINES AHEAD
		regDataADT never_Be_The_Same;
		never_Be_The_Same.fileToADT(filename);
		never_Be_The_Same.SortTheADT();              //the file gets sorted in "phaseb.csv"

		ifstream fin; fin.open("phaseb.csv");

		if (fin.is_open())
		{
			//cout << filename << "\n\n\nOpened Successfully\n"; //setColor(8);

			char c[100];
			fin.getline(c, 100, '\n'); // ----------------->   1st row skipped :3

			char RN1[100]; char Name1[100]; char CN1[100]; char S1[100];
			char RN2[100]; char Name2[100]; char CN2[100]; char S2[100];

			fin.getline(RN1, 100, ',');   //cout << RN1 << "\t";    //rollnumber
			fin.getline(Name1, 100, ',');  //cout << Name1 << "\t"; //Name

			fin.getline(CN1, 150, ',');    //cout << CN1 << "\t";   //Course_name
			fin.getline(S1, 100, '\n');    //cout << S1 << "\n";    //Section

			int ii = 0;
			while (!fin.eof())
			{
				//cout << "\n--------------NEW_CLASSLIST_OBJECT :) -----------------\n";
				//cout << RN1 << " " << Name1 << " " << CN1 << " " << S1 << endl; //<<<this is being inserted


				classlist * ptr = new classlist; //<==============================NEW_CLASSLIST_OBJECT 
				ptr->count = 0;

				ptr->names[ptr->count] = GetStringFromBuffer(Name1);
				ptr->rollNo[ptr->count] = GetStringFromBuffer(RN1);
				ptr->count += 1; //ESSENTIAL !!

								 //cout <<ii++<<" "<< ptr->count << " " << ptr->names[ptr->count] << " " << ptr->rollNo[ptr->count] << "\n";//<<<<<<<<<<<<<<<<<



				bool thesame_course = true;
				while (thesame_course)
				{
					fin.getline(RN2, 100, ',');
					fin.getline(Name2, 100, ',');
					fin.getline(CN2, 150, ',');
					fin.getline(S2, 100, '\n');   //cout << RN2 << "\t" << Name2 << "\t"<< CN2 << "\t"<< S2 << "\n";


					if (strcmp(CN1, CN2) == 0 && strcmp(S1, S2) == 0) //Both belong to the same course
					{
						//cout << RN2 << " " << Name2 << " " << CN2 << " " << S2 << endl;

						ptr->names[ptr->count] = GetStringFromBuffer(Name2);//1)
						ptr->rollNo[ptr->count] = GetStringFromBuffer(RN2);//1)

																		   //cout <<"\t"<<ii++<<" "<<ptr->count<<" "<< ptr->names[ptr->count] << " " << ptr->rollNo[ptr->count] << "\t";//<<<<<<<<<<<<<<<<<

						ptr->count += 1;
					}
					else
					{
						Search_in_TTADT_plus_do_work_(CN1, S1, ptr); //SEARCH THE COURSE IN TTADT AND FIT EM IN

						strcpy_s(RN1, RN2);
						strcpy_s(Name1, Name2);
						strcpy_s(CN1, CN2);
						strcpy_s(S1, S2);
						thesame_course = false;
					}
				}
			}
		}
		else
			cout << "DIDNOT OPEN -_-";
	}

	//------------------------------------PHASE (C) METHODS---------------------------------------------
	//------------------------------C(1)-----------------------------------

	void clashsaver(hlp w[], int count) {

		ofstream fout;
		fout.open("clash.txt");

		hlp arr[10];
		int clashcount = 0;
		int kount = 0;
		int c = 0;
		int i = 0;
		char* r1;
		char* r2;
		r1 = w[c].roll;
		i++;
		r2 = w[c + 1].roll;
		i++;
		for (i; i < 6726; )
		{
			arr[kount] = w[c];
			kount++;
			c++;
			while (strcmp(r1, r2) == 0) {
				arr[kount] = w[c];
				i++;
				kount++;
				c++;
				r2 = w[c + 1].roll;
			}
			char*c;
			char* s;
			char* st;
			char* et;
			char* st1;
			char* st2;
			char* et1;
			char* et2;


			//For Monday
			for (int j = 0; j < kount; j++)
			{
				c = arr[j].course;
				s = arr[j].section;
				st = Monday.Roomlist.search_and_set(c, s);
				arr[j].st_time = st;
				et = Monday.Roomlist.search_and_set2(c, s);
				arr[j].end_time = et;
			}
			for (int k = 0; k < kount; k++)
			{
				st1 = arr[k].st_time;
				et1 = arr[k].end_time;
				for (int l = k + 1; l < kount; l++)
				{

					st2 = arr[l].st_time;

					et2 = arr[l].end_time;
					if (st1 != NULL && st2 != NULL)
					{
						if (strcmp(st1, st2) == 0 || strcmp(et1, et2) == 0) {
							clashcount++;
							fout << arr[k].roll << " " << arr[k].name << " ";
							fout << arr[k].course << " " << arr[k].section << " with ";
							fout << arr[l].course << " " << arr[l].section;
							fout << " on Monday at " << arr[k].st_time << endl;
						}
					}
				}
			}

			//For Tuesday
			for (int j = 0; j < kount; j++)
			{
				c = arr[j].course;
				s = arr[j].section;
				st = Tuesday.Roomlist.search_and_set(c, s);
				arr[j].st_time = st;
				et = Tuesday.Roomlist.search_and_set2(c, s);
				arr[j].end_time = et;
			}
			for (int k = 0; k < kount; k++)
			{
				st1 = arr[k].st_time;
				et1 = arr[k].end_time;
				for (int l = k + 1; l < kount; l++)
				{

					st2 = arr[l].st_time;

					et2 = arr[l].end_time;
					if (st1 != NULL && st2 != NULL)
					{
						if (strcmp(st1, st2) == 0 || strcmp(et1, et2) == 0) {
							clashcount++;
							fout << arr[k].roll << " " << arr[k].name << " ";
							fout << arr[k].course << " " << arr[k].section << " with ";
							fout << arr[l].course << " " << arr[l].section;
							fout << " on Tuesday at " << arr[k].st_time << endl;
						}
					}
				}
			}

			//For Wednesday
			for (int j = 0; j < kount; j++)
			{
				c = arr[j].course;
				s = arr[j].section;
				st = Wednesday.Roomlist.search_and_set(c, s);
				arr[j].st_time = st;
				et = Wednesday.Roomlist.search_and_set2(c, s);
				arr[j].end_time = et;
			}
			for (int k = 0; k < kount; k++)
			{
				st1 = arr[k].st_time;
				et1 = arr[k].end_time;
				for (int l = k + 1; l < kount; l++)
				{

					st2 = arr[l].st_time;

					et2 = arr[l].end_time;
					if (st1 != NULL && st2 != NULL)
					{
						if (strcmp(st1, st2) == 0 || strcmp(et1, et2) == 0) {
							clashcount++;
							fout << arr[k].roll << " " << arr[k].name << " ";
							fout << arr[k].course << " " << arr[k].section << " with ";
							fout << arr[l].course << " " << arr[l].section;
							fout << " on Wednesday at " << arr[k].st_time << endl;
						}
					}
				}
			}

			//For Thursday
			for (int j = 0; j < kount; j++)
			{
				c = arr[j].course;
				s = arr[j].section;
				st = Thursday.Roomlist.search_and_set(c, s);
				arr[j].st_time = st;
				et = Thursday.Roomlist.search_and_set2(c, s);
				arr[j].end_time = et;
			}
			for (int k = 0; k < kount; k++)
			{
				st1 = arr[k].st_time;
				et1 = arr[k].end_time;
				for (int l = k + 1; l < kount; l++)
				{

					st2 = arr[l].st_time;

					et2 = arr[l].end_time;
					if (st1 != NULL && st2 != NULL)
					{
						if (strcmp(st1, st2) == 0 || strcmp(et1, et2) == 0) {
							clashcount++;
							fout << arr[k].roll << " " << arr[k].name << " ";
							fout << arr[k].course << " " << arr[k].section << " with ";
							fout << arr[l].course << " " << arr[l].section;
							fout << " on Thursday at " << arr[k].st_time << endl;
						}
					}
				}
			}

			//For Friday
			for (int j = 0; j < kount; j++)
			{
				c = arr[j].course;
				s = arr[j].section;
				st = Friday.Roomlist.search_and_set(c, s);
				arr[j].st_time = st;
				et = Friday.Roomlist.search_and_set2(c, s);
				arr[j].end_time = et;
			}
			for (int k = 0; k < kount; k++)
			{
				st1 = arr[k].st_time;
				et1 = arr[k].end_time;
				for (int l = k + 1; l < kount; l++)
				{

					st2 = arr[l].st_time;

					et2 = arr[l].end_time;
					if (st1 != NULL && st2 != NULL)
					{
						if (strcmp(st1, st2) == 0 || strcmp(et1, et2) == 0) {
							clashcount++;
							fout << arr[k].roll << " " << arr[k].name << " ";
							fout << arr[k].course << " " << arr[k].section << " with ";
							fout << arr[l].course << " " << arr[l].section;
							fout << " on Friday at " << arr[k].st_time << endl;
						}
					}
				}
			}




			// reset for next set of courses
			kount = 0;
			r1 = r2;
		}
		cout << endl << endl << endl << endl;
		cout << clashcount;
		fout.close();
	}
	void saveClashes() {
		ifstream fin;
		fin.open("phasec.csv");

		hlp clash[10000];
		int count = 0;
		char x[100];

		char *r;
		char* n;
		char* c;
		char* s;

		fin.ignore(50, '\n');
		while (!fin.eof()) {

			fin.getline(x, 100, ',');		r = GetStringFromBuffer(x);

			fin.getline(x, 100, ',');		n = GetStringFromBuffer(x);

			fin.getline(x, 100, ',');		c = GetStringFromBuffer(x);

			fin.getline(x, 100, '\n');		s = GetStringFromBuffer(x);

			clash[count].roll = r;
			clash[count].name = n;
			clash[count].course = c;
			clash[count].section = s;

			count++;
		}
		clashsaver(clash, count);
 	}

	//-------------------------------C(2)-------------------------------------
	void printCourseTimings0(const char * coursename, const char *section) //private me plz
	{
		Monday.Roomlist.Find5_(coursename, section, Monday.name);
		Tuesday.Roomlist.Find5_(coursename, section, Tuesday.name);
		Wednesday.Roomlist.Find5_(coursename, section, Wednesday.name);
		Thursday.Roomlist.Find5_(coursename, section, Thursday.name);
		Friday.Roomlist.Find5_(coursename, section, Friday.name);//Can Be Optimized but is fast regardless :)
	}

	void printStudentTimeTable(const char * rollno)
	{
		ifstream fin;
		fin.open("phasec.csv");
		if (fin.is_open()) {
			cout << "phase c opened" << endl;
		}
		else {
			cout << "phase c file not opened" << endl;
		}
		hlp courses[10];
		int count = 0;
		char x[100];

		//node 1
		char*r1;
		char*n1;
		char*c1;
		char*s1;

		fin.ignore(50, '\n');



		while (!fin.eof()) {
			fin.getline(x, 100, ',');		r1 = GetStringFromBuffer(x);

			fin.getline(x, 100, ',');		n1 = GetStringFromBuffer(x);

			fin.getline(x, 100, ',');		c1 = GetStringFromBuffer(x);

			fin.getline(x, 100, '\n');		s1 = GetStringFromBuffer(x);
			while (strcmp(r1, rollno) == 0) {

				courses[count].roll = r1;
				courses[count].name = n1;
				courses[count].course = c1;
				courses[count].section = s1;
				count++;

				fin.getline(x, 100, ',');		r1 = GetStringFromBuffer(x);

				fin.getline(x, 100, ',');		n1 = GetStringFromBuffer(x);

				fin.getline(x, 100, ',');		c1 = GetStringFromBuffer(x);

				fin.getline(x, 100, '\n');		s1 = GetStringFromBuffer(x);

			}
		}
		char*c;
		char*s;
		char*t1;
		char*t2;
		char*r;

		for (int i = 0; i < count; i++)
		{
			c = courses[i].course;
			s = courses[i].section;
			if (Monday.Roomlist.inTTADT(c, s) == TRUE || Tuesday.Roomlist.inTTADT(c, s) == TRUE || Wednesday.Roomlist.inTTADT(c, s) == TRUE || Thursday.Roomlist.inTTADT(c, s) == TRUE || Friday.Roomlist.inTTADT(c, s) == TRUE) {
				cout << endl;
				cout << c << " (" << s << ")" << endl;
				//Monday
				r = Monday.Roomlist.GetRoomName(c, s);
				courses[i].room = r;

				t1 = Monday.Roomlist.search_and_set(c, s);
				courses[i].st_time = t1;

				t2 = Monday.Roomlist.search_and_set2(c, s);
				courses[i].end_time = t2;

				if (courses[i].st_time != NULL) {
					cout << Monday.name << " - " << courses[i].st_time << " : " << courses[i].end_time << " " << courses[i].room << "||";
				}

				//Tuesday
				r = Tuesday.Roomlist.GetRoomName(c, s);
				courses[i].room = r;

				t1 = Tuesday.Roomlist.search_and_set(c, s);
				courses[i].st_time = t1;

				t2 = Tuesday.Roomlist.search_and_set2(c, s);
				courses[i].end_time = t2;

				if (courses[i].st_time != NULL) {
					cout << Tuesday.name << " - " << courses[i].st_time << " : " << courses[i].end_time << " " << courses[i].room << "||";
				}

				//Wednesday
				r = Wednesday.Roomlist.GetRoomName(c, s);
				courses[i].room = r;

				t1 = Wednesday.Roomlist.search_and_set(c, s);
				courses[i].st_time = t1;

				t2 = Wednesday.Roomlist.search_and_set2(c, s);
				courses[i].end_time = t2;

				if (courses[i].st_time != NULL) {
					cout << Wednesday.name << " - " << courses[i].st_time << " : " << courses[i].end_time << " " << courses[i].room << "||";
				}

				//Thursday
				r = Thursday.Roomlist.GetRoomName(c, s);
				courses[i].room = r;

				t1 = Thursday.Roomlist.search_and_set(c, s);
				courses[i].st_time = t1;

				t2 = Thursday.Roomlist.search_and_set2(c, s);
				courses[i].end_time = t2;

				if (courses[i].st_time != NULL) {
					cout << Thursday.name << " - " << courses[i].st_time << " : " << courses[i].end_time << " " << courses[i].room << "||";
				}

				//Friday
				r = Friday.Roomlist.GetRoomName(c, s);
				courses[i].room = r;

				t1 = Friday.Roomlist.search_and_set(c, s);
				courses[i].st_time = t1;

				t2 = Friday.Roomlist.search_and_set2(c, s);
				courses[i].end_time = t2;

				if (courses[i].st_time != NULL) {
					cout << Friday.name << " - " << courses[i].st_time << " : " << courses[i].end_time << " " << courses[i].room << "||";
				}
			}
			cout << endl;
		}




	}

	//-------------------------------C(3)----------------------------------------
	void printStudentCourses(const char * rollno)	//<SUBJECT> <SECTION>
	{
		ifstream fin;
		fin.open("phasec.csv");
		if (fin.is_open()) {
			cout << "phase c opened" << endl;
		}
		else {
			cout << "phase c file not opened" << endl;
		}
		hlp courses[10];
		int count = 0;
		char x[100];

		//node 1
		char*r1;
		char*n1;
		char*c1;
		char*s1;

		fin.ignore(50, '\n');



		while (!fin.eof()) {
			fin.getline(x, 100, ',');		r1 = GetStringFromBuffer(x);

			fin.getline(x, 100, ',');		n1 = GetStringFromBuffer(x);

			fin.getline(x, 100, ',');		c1 = GetStringFromBuffer(x);

			fin.getline(x, 100, '\n');		s1 = GetStringFromBuffer(x);
			while (strcmp(r1, rollno) == 0) {

				courses[count].roll = r1;
				courses[count].name = n1;
				courses[count].course = c1;
				courses[count].section = s1;
				count++;

				fin.getline(x, 100, ',');		r1 = GetStringFromBuffer(x);

				fin.getline(x, 100, ',');		n1 = GetStringFromBuffer(x);

				fin.getline(x, 100, ',');		c1 = GetStringFromBuffer(x);

				fin.getline(x, 100, '\n');		s1 = GetStringFromBuffer(x);

			}
		}
		cout << rollno << "	" << courses[0].name << endl;
		char* c;
		char* s;
		for (int i = 0; i < count; i++)
		{
			c = courses[i].course;
			s = courses[i].section;
			if (Monday.Roomlist.inTTADT(c, s) == TRUE || Tuesday.Roomlist.inTTADT(c, s) == TRUE || Wednesday.Roomlist.inTTADT(c, s) == TRUE || Thursday.Roomlist.inTTADT(c, s) == TRUE || Friday.Roomlist.inTTADT(c, s) == TRUE) {
				cout << courses[i].course << "	" << courses[i].section << endl;
			}
		}
	}

	//----------------------------------C(4)----------------------------------------
	void printCourse(const char * day, const char *time, const char *classroom) //sir ka functiom
	{

		if (strcmp(day, "Monday") == 0 || strcmp(day, "monday") == 0)
			Monday.Roomlist.Find4(time, classroom);
		else
			if (strcmp(day, "Tuesday") == 0 || strcmp(day, "tuesday") == 0)
				Tuesday.Roomlist.Find4(time, classroom);
			else if (strcmp(day, "Wednesday") == 0 || strcmp(day, "wednesday") == 0)
				Wednesday.Roomlist.Find4(time, classroom);
			else if (strcmp(day, "Thursday") == 0 || strcmp(day, "thursday") == 0)
				Thursday.Roomlist.Find4(time, classroom);
			else if (strcmp(day, "Friday") == 0 || strcmp(day, "friday") == 0)
				Friday.Roomlist.Find4(time, classroom);
	}

	//-----------------------------------C(5)-------------------------------------------
	void printCourseTimings(const char * coursename, const char *section)
	{
		Monday.Roomlist.Find5(coursename, section, Monday.name);
		Tuesday.Roomlist.Find5(coursename, section, Tuesday.name);
		Wednesday.Roomlist.Find5(coursename, section, Wednesday.name);
		Thursday.Roomlist.Find5(coursename, section, Thursday.name);
		Friday.Roomlist.Find5(coursename, section, Friday.name);//Can Be Optimized but is fast regardless :)
	}


};
//-------------------------END OF TTADT----------------------------------------------//



//++++++++++++++
//+++ MAIN() +++
//++++++++++++++
int main() {


	//------------------------------------------------------PHASE-A-----------------------------------------------------------------------//
	TimeTable neon;
	neon.Load("SOS.csv");

	//------------------------------------------------------PHASE-B-----------------------------------------------------------------------//  
	neon.loadStudentInfo("regdata.csv");
	cout << endl;
	system("cls");
	setColor(13);

	//------------------------------------------------------PHASE-C-----------------------------------------------------------------------//

	//C1
	neon.saveClashes();

	//C2
	//neon.printStudentTimeTable("1");
	neon.printStudentTimeTable("47");
	cout << endl << endl << endl << endl;

	//C3
	neon.printStudentCourses("199");
	cout << endl << endl << endl << endl;
	neon.printStudentCourses("1280");
	neon.printStudentTimeTable("47");
	cout << endl << endl << endl << endl;

	//C4
	neon.printCourse("Monday", "2:00", "CS-2");
	cout << endl << endl << endl << endl;

	//C5
	neon.printCourseTimings("Human Computer Interaction", "CS-C");
	cout << endl << endl << endl << endl;
	//----------------------------------------------------------------------------------------------------------------------------------//


	system("PAUSE");
}