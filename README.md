# PluzzeGame
// Đây lớp viên gạch hình chữ I
**Gach_I.h**
#pragma once

class Gach_I:public HinhDang
{
public:
	Gach_I(void);
	~Gach_I(void);
	void layhinh(int a[][4],int &dong,int &cot,int &Row,int &Col,int loai);
};

**GachI.cpp**
#include "StdAfx.h"
#include "Gach_I.h"

Gach_I::Gach_I(void)
{
}

Gach_I::~Gach_I(void)
{
}
//viên gạch hình chữ I nằm ngang dài 4 ô, rộng 1; xuất phát từ dòng 19, cột 6
void Gach_I::layhinh(int a[][4],int &dong,int &cot,int &Row,int &Col,int loai)
{
   dong=19;
   cot=6;
   Row=4;
   Col=1;
   for(int i=0;i < Row ;i++)
  {
     a[i][0]=1;
  }
}

**Gach_LZT.h**
#pragma once

class Gach_LZT:public HinhDang
{
public:
	Gach_LZT(void);
	~Gach_LZT(void);
	void layhinh(int a[][4],int &dong,int &cot,int &Row,int &Col,int loai);
};
**Gach_LZT.cpp**
#include "StdAfx.h"
#include "Gach_LZT.h"

Gach_LZT::Gach_LZT(void)
{
}

Gach_LZT::~Gach_LZT(void)
{
}
//viên gạch hình chữ LZT nằm ngang dài 2 ô, rộng 3; xuất phát từ dòng 21, cột 6
void Gach_LZT::layhinh(int a[][4],int &dong,int &cot,int &Row,int &Col,int loai)
{
			Row=2;
			Col=3;
			dong=21;
			cot=6;
                        //ra ma trận hình L,Z,T tùy vào biến loai
			for(int i=0;i< Row*Col ;i++)
			{
				a[i/3][i%3]=(loai>>(5-i))&1;//dich bit
			}
}
**Gach_U.h**
#pragma once

class Gach_U:public HinhDang
{
public:
	Gach_U(void);
	~Gach_U(void);
	void layhinh(int a[][4],int &dong,int &cot,int &Row,int &Col,int loai);
};
**Gach_U.cpp**
#include "StdAfx.h"
#include "Gach_U.h"

Gach_U::Gach_U(void)
{
}

Gach_U::~Gach_U(void)
{
}
//viên gạch hình chữ U nằm ngang dài 2 ô, rộng 2; xuất phát từ dòng 21, cột 6
void Gach_U::layhinh(int a[][4],int &dong,int &cot,int &Row,int &Col,int loai)
{
			dong=21;
			cot=6;
			Row=Col=2;
                     //vẽ ma trân hình vuông
			for(int i=0; i < Row;i++)
			{
				for(int j=0; j < Col;j++)
				   a[i][j]=1;
			}
}
**HinhDang.h**
#pragma once

class HinhDang
{
public:
	
	HinhDang(void);
	~HinhDang(void);
	virtual void layhinh(int a[][4],int &dong,int &cot,int &Row,int &Col,int loai);//lay hinh dang cua khoi gach xac dinh loai de ve hinh dang
};
**HinhDang.cpp**
#include "StdAfx.h"
#include "HinhDang.h"

HinhDang::HinhDang(void)
{
}

HinhDang::~HinhDang(void)
{
}
void HinhDang::layhinh(int a[][4],int &dong,int &cot,int &Row,int &Col,int loai)
{
	return;
}
**imageloader.cpp**

//đây là lớp dùng để load hình, như là load ảnh nền, ảnh viên gạch, tường...
#include "stdafx.h"
#include 
#include 

#include "imageloader.h"

using namespace std;

Image::Image(char* ps, int w, int h) : pixels(ps), width(w), height(h) {
	
}

Image::~Image() {
	delete[] pixels;
}

namespace {
	//Converts a four-character array to an integer, using little-endian form
	int toInt(const char* bytes) {
		return (int)(((unsigned char)bytes[3] << 24) |
					 ((unsigned char)bytes[2] << 16) |
					 ((unsigned char)bytes[1] << 8) |
					 (unsigned char)bytes[0]);
	}
	
	//Converts a two-character array to a short, using little-endian form
	short toShort(const char* bytes) {
		return (short)(((unsigned char)bytes[1] << 8) |
					   (unsigned char)bytes[0]);
	}
	
	//Reads the next four bytes as an integer, using little-endian form
	int readInt(ifstream &input) {
		char buffer[4];
		input.read(buffer, 4);
		return toInt(buffer);
	}
	
	//Reads the next two bytes as a short, using little-endian form
	short readShort(ifstream &input) {
		char buffer[2];
		input.read(buffer, 2);
		return toShort(buffer);
	}
	
	//Just like auto_ptr, but for arrays
	template<class T>
	class auto_array {
		private:
			T* array;
			mutable bool isReleased;
		public:
			explicit auto_array(T* array_ = NULL) :
				array(array_), isReleased(false) {
			}
			
			auto_array(const auto_array &aarray) {
				array = aarray.array;
				isReleased = aarray.isReleased;
				aarray.isReleased = true;
			}
			
			~auto_array() {
				if (!isReleased && array != NULL) {
					delete[] array;
				}
			}
			
			T* get() const {
				return array;
			}
			
			T &operator*() const {
				return *array;
			}
			
			void operator=(const auto_array &aarray) {
				if (!isReleased && array != NULL) {
					delete[] array;
				}
				array = aarray.array;
				isReleased = aarray.isReleased;
				aarray.isReleased = true;
			}
			
			T* operator->() const {
				return array;
			}
			
			T* release() {
				isReleased = true;
				return array;
			}
			
			void reset(T* array_ = NULL) {
				if (!isReleased && array != NULL) {
					delete[] array;
				}
				array = array_;
			}
			
			T* operator+(int i) {
				return array + i;
			}
			
			T &operator[](int i) {
				return array[i];
			}
	};
}

Image* loadBMP(const char* filename) {
	ifstream input;
	input.open(filename, ifstream::binary);
	assert(!input.fail() || !"Could not find file");
	char buffer[2];
	input.read(buffer, 2);
	assert(buffer[0] == 'B' && buffer[1] == 'M' || !"Not a bitmap file");
	input.ignore(8);
	int dataOffset = readInt(input);
	
	//Read the header
	int headerSize = readInt(input);
	int width;
	int height;
	switch(headerSize) {
		case 40:
			//V3
			width = readInt(input);
			height = readInt(input);
			input.ignore(2);
			assert(readShort(input) == 24 || !"Image is not 24 bits per pixel");
			assert(readShort(input) == 0 || !"Image is compressed");
			break;
		case 12:
			//OS/2 V1
			width = readShort(input);
			height = readShort(input);
			input.ignore(2);
			assert(readShort(input) == 24 || !"Image is not 24 bits per pixel");
			break;
		case 64:
			//OS/2 V2
			assert(!"Can't load OS/2 V2 bitmaps");
			break;
		case 108:
			//Windows V4
			assert(!"Can't load Windows V4 bitmaps");
			break;
		case 124:
			//Windows V5
			assert(!"Can't load Windows V5 bitmaps");
			break;
		default:
			assert(!"Unknown bitmap format");
	}
	
	//Read the data
	int bytesPerRow = ((width * 3 + 3) / 4) * 4 - (width * 3 % 4);
	int size = bytesPerRow * height;
	auto_array<char> pixels(new char[size]);
	input.seekg(dataOffset, ios_base::beg);
	input.read(pixels.get(), size);
	
	//Get the data into the right format
	auto_array<char> pixels2(new char[width * height * 3]);
	for(int y = 0; y < height; y++) {
		for(int x = 0; x < width; x++) {
			for(int c = 0; c < 3; c++) {
				pixels2[3 * (width * y + x) + c] =
					pixels[bytesPerRow * y + 3 * x + (2 - c)];
			}
		}
	}
	
	input.close();
	return new Image(pixels2.release(), width, height);
}

**imageloader.h**


#ifndef IMAGE_LOADER_H_INCLUDED
#define IMAGE_LOADER_H_INCLUDED

//Represents an image
class Image {
	public:
		Image(char* ps, int w, int h);
		~Image();
		
		/* An array of the form (R1, G1, B1, R2, G2, B2, ...) indicating the
		 * color of each pixel in image.  Color components range from 0 to 255.
		 * The array starts the bottom-left pixel, then moves right to the end
		 * of the row, then moves up to the next column, and so on.  This is the
		 * format in which OpenGL likes images.
		 */
		char* pixels;
		int width;
		int height;
};

//Reads a bitmap image from file.
Image* loadBMP(const char* filename);










#endif

**KhoiGach.h**
#pragma once

class KhoiGach
{
private:
		int a[4][4];//ma trận đơn vị bật bit 1 để vẽ, còn lại bit 0
		VienGach Khoi[4];//một khối gạch gồm có 4 viên gạch
		int Row,Col;//chỉ số dòng cột của ma trận đơn vị, là viên gạch
	 	int dong,cot;//chỉ số dòng cột của ma trận lớn, là bàn cờ
	
public:
	KhoiGach(int loai,int xxx);//xác định hình dạng thông qua biến loai, xxx neu la 0 thi ve khoi gach trong bang, 
	//neu xxx la 1 thi ve khoi gach se xuat hien trong lan chay tiep theo xuat o ngoai bang
	void noshow();//khong ve khoi gach 
	void show( GLuint _textureId,int score, int level,int &flag);//ve khoi gach ve diem, cap do choi
	void dichtrai(int board[][maxj] );// dich khoi gach sang trai
	void dichphai(int board[][maxj]);
	int dichxuong(int board[][maxj]);
	void xoaykhoi();//xoay khoi gach
	void codinhkhoi(int board[][maxj]);//co dinh khoi gach
	void KiemTra(int board[][maxj],GLuint _textureId,int &score,int &level,int &flag);//kiem tra thang thua, cap nhat diem de qua level,
	//cap nhap bang neu dong nao chua toan gach va khong co cho trong thi se ghi diem xoa di tat ca nhung vien gach thuoc dong do va cho
	//nhung dong o tren roi xuong lap dong vua bi mat.
};

**KhoiGach.cpp**
#include "StdAfx.h"
#include "KhoiGach.h"
//lớp này xác định hình dạng viên gạch và load hình viên gạch lên màn hình
KhoiGach::KhoiGach(int loai,int xxx)
{
	int i,j,stt;
	HinhDang *temp;
		for(i=0;i<4;i++)
			for(j=0;j<4;j++)
				a[i][j]=0;
		switch(loai)
		{
			case 15:
				temp=new Gach_I;
					break;
			case 31:
				temp=new Gach_U;
				break;
			default:
				temp=new Gach_LZT;
				break;
		}
		temp->layhinh(a,dong,cot,Row,Col,loai);
		if(xxx==0)//ve trog khung
		{
			stt=0;
			for(i=0;i//4->0,1,2,3
			{	for(j=0;j//thang dung  1->j=0
				{
					//printf("%d",a[i][j]);
					if(a[i][j]==1)
					{
						//printf("%d\n",stt);
						Khoi[stt].setinfo((j+cot)*25+left,(i+dong)*25+bottom);//truyen vao toa do can ve~
						stt++;
					}
				}
			//printf("\n");
			}
		}
		if(xxx==1)//de ve khoi next ngoai khung
		{
			stt=0;
			for(i=0;i//4->0,1,2,3
			{	for(j=0;j//thang dung  1->j=0
				{
					if(a[i][j]==1)
					{
						Khoi[stt].setinfo((j+6)*25+right,(i+14)*25+bottom);//truyen vao toa do can ve~
						stt++;
					}
				}
			}
		}
}
//không hiển thị
void KhoiGach::noshow()
{
		for(int i=0;i<4;i++)
			Khoi[i].nodisplay();
}
//hiển thị viên gạch
void KhoiGach::show(GLuint _textureId,int score,int level,int &flag)
{
		for(int i=0;i<4;i++)
			Khoi[i].display( _textureId,score,level,flag);//ve 4 cuc gach cua 1 khoi gach
}
//dịch sang bên trái
void KhoiGach::dichtrai(int board[][maxj] )
	{
		for(int i=0;i<4;i++)
			if(Khoi[i].checkleft(board)==0)//neu dich trai hok dc
				return;
		for(int i=0;i<4;i++)
			Khoi[i].moveleft();
		cot=cot-1;
	}
	void KhoiGach::dichphai(int board[][maxj])
	{
		for(int i=0;i<4;i++)
			if(Khoi[i].checkright(board)==0)
				return;
		for(int i=0;i<4;i++)
			Khoi[i].moveright();
		cot=cot+1;
	}
//dịch xuống
int KhoiGach::dichxuong(int board[][maxj])
	{
		int check=1;
			for(int i=0;i<4;i++)
			{
				//printf("%d",Khoi[i].checkdown());
				if(Khoi[i].checkdown(board)==0)//mot trong 4 cuc hok xuong dc
					check=0;
						continue;
			}
		if(check==1)//neu di chuyen dc
		{
			for(int i=0;i<4;i++)
				Khoi[i].movedown();//cho 4 cuc di xuong
			dong=dong-1;//dong giam di 1
			return 1;//tra ket qua dich xuong dc
		}
			return 0;//nguoc lai hok dich xuong dc
	}
//xoay khối gạch
void KhoiGach::xoaykhoi()
	{
		int i,j,stt=0;
		int b[4][4];
		int tempRow=this->Col,tempCol=this->Row;
		for(i=0;i<4;i++)
			for(j=0;j<4;j++)
				b[i][j]=0;
		for(i=Row-1;i>=0;i--)//chuyen doi khoi,[3][0]->[0][3],[2][0]->[0][2],[1][0]->[0][1],[0][0]->[0][0]
			for(j=Col-1;j>=0;j--)
			{   
				b[j][Row-i-1]=a[i][j];
			}
		Row=tempRow;
		Col=tempCol;
		for(i=0;i//kiem tra xoay dc hok,neu hok thi return no ngay
			for(j=0;jif(b[i][j]==1)
					if(j+cot>=right-25)
					return;
			}
		stt=0;
		for(i=0;ifor(j=0;jif(a[i][j]==1)
			{
				Khoi[stt].setinfo((j+cot)*25+left,(i+dong)*25+bottom);
				stt++;
			}
		}
	}
//cố định khối gạch, dùng để khi đến đáy 
void KhoiGach::codinhkhoi(int board[][maxj])
	{
		if(dichxuong(board)==0)//neu dich xuong hok dc
		{
			for(int i=0;i<4;i++)
				Khoi[i].Khoa(board);//khoa tat ca khoi gach lai bang cach bat ma tran len 1 va block 1
		}
	}
//kiểm tra xem có kín hàng chưa, nếu có thì cho nổ
void KhoiGach::KiemTra(int board[][maxj],GLuint _textureId,int &score,int &level,int &flag)
{
			int count=0;
			VienGach temp,t;
				for(int j=2;jif(board[21][j]==1)
					{
							int diem;
							FILE *f2=fopen("output.txt","rt");
							while(!feof(f2))
							{
								fscanf(f2,"%d",&diem);
							}
							printf("%d",diem);
							FILE *f1=fopen("output.txt","wt");
							
							if(diemfprintf(f1,"%d",score);
							else
								fprintf(f1,"%d",diem);
						PlaySound(L"GAMEOVER.wav", NULL,  SND_ASYNC);
						flag=-2;
						score=0;
						level=0;
						for(int i=3;i < maxi;i++)
							for(int j=2;j < maxj;j++)
							{
								board[i][j]=0;
								temp.setinfo(j*25+left,(i-2)*25+bottom);
								temp.nodisplay();
							}
							
					}

				}
			for(int i=3; i < maxi;i++)
			{
				for(int j=2; j < maxj;j++)
				{
					if(board[i][j]==1)
						count++;
					if(count==10)
					{

						
						score=score+10;
						if(score>=level*50)
						{
							level=level+1;
							MessageBox(NULL,L"Chuc Mung Ban Qua 1 Level",L"Thong Bao",MB_OK);
							if(level==9)
								level=1;
							PlaySound(L"next_level.wav", NULL,SND_ASYNC);
							PlaySound(L"2.wav", NULL,SND_ASYNC );
						}
						for(int j=2; j < maxj;j++)
						{
							temp.setinfo(j*25+left,(i-2)*25+bottom);
							temp.nodisplay();
						}
						for(int ki=i; ki < maxi-2;ki++)
							for(int kj=2; kj < maxj;kj++)
							{
								
									if(board[ki+1][kj]==1)
									{
										t.setinfo(kj*25+left,(ki-2)*25+bottom);
										t.display(_textureId,score,level,flag);
									}
									if(board[ki+1][kj]==0)
									{
										t.setinfo(kj*25+left,(ki-2)*25+bottom);
										t.nodisplay();
									}
							}
						for(int ki=i; ki < maxi-2;ki++)
							for(int kj=2; kj < maxj;kj++)
							{
								
									board[ki][kj]=board[ki+1][kj];
								
							}
							PlaySound(L"bom.wav", NULL,  SND_ASYNC);
							PlaySound(L"2.wav", NULL,SND_ASYNC );
							count=0;
							//return;
					}
				}
				
				count=0;
			}
}

**VienGach.h**
#pragma once

class VienGach
{
private:
		int x,y;
		int block;//khi vien gach cham day hoac cham nhung vien gach khac o ben duoi thi block=1 vien gach se duoc co dinh lai
		int font;//ve text
		char s[40];//chuoi cua score
		char s1[40];//chuoi level
public:
	VienGach();
	void setinfo(int i,int j);//lay toa do de ve vien gach
	void moveleft();//di chuyen vien gach sang trai
	void moveright();
	void movedown();
	int checkleft(int board[][maxj]);//kiem tra vien gach co sang trai duoc hay khong neu duoc thi tra ve gia tri 1 nguoc lai la 0
	int checkright(int board[][maxj]);
	int checkdown(int board[][maxj]);
	void display(GLuint _textureId,int score,int level,int &flag);//ve vien gach ve diem so, cap do 
	void nodisplay();//xoa vien gach 
	void Khoa(int board[][maxj]);//khoa vien gach lai block=1 va ma tran ngay cho vien gach hien ra =1
	GLuint loadTexture(Image* image);//load hinh
	void setOrthographicProjection();// ve hinh
	void renderBitmapString(float x, float y, void *font,char *string);//ve chuoi

};

**VienGach.cpp**
#include "StdAfx.h"
#include "VienGach.h"

		VienGach::VienGach()
		{
			x=y=block=0;
			font=(int)GLUT_BITMAP_TIMES_ROMAN_24;
		}
		void VienGach::setinfo(int i,int j)
		{
			x=i;//toa do cot
			y=j;//toa do dong
			//printf("x%d y%d\n",x,y);
		}
		void VienGach::moveleft()
		{
			x=x-25;
		}
		void VienGach::moveright()
		{
			x=x+25;
		}
		void VienGach::movedown()
		{
			y=y-25;
		}
		int VienGach::checkleft(int board[][maxj])
		{
			if(y>bottom&&x>left&&board[(y-bottom)/25][(x-left-25)/25]==0&&block==0)
				return 1;
			return 0;
		}
		int VienGach::checkright(int board[][maxj])
		{
			if(y > bottom && x < right && board[(y-bottom)/25][(x-left+25)/25]==0 && block==0)
				return 1;
			return 0;
		}
		int VienGach::checkdown(int board[][maxj])//kiem tra xuong dc nua hok
		{
			//printf("board[%d][%d]=%d\n",(y-bottom+25)/25,(x-left)/25,board[(y-bottom+25)/25][(x-left)/25]);
			if(y < top && board[(y-bottom+25)/25][(x-left)/25]==0 && block==0)
				return 1;
			return 0;
		}
void VienGach::setOrthographicProjection()
{

	
	glMatrixMode(GL_PROJECTION);
	glPushMatrix();
	glLoadIdentity();
	gluOrtho2D(0,screenWidth,0,screenHeight);
	//glMatrixMode(GL_MODELVIEW);
}
void VienGach::renderBitmapString(float x, float y, void *font,char *string)
{
  
  char *c;
  glRasterPos2f(x, y);
  for (c=string; *c != '\0'; c++)
  {
    glutBitmapCharacter(font, *c);
  }
}

void VienGach::display(GLuint _textureId,int score,int level,int &flag)
	{
			
			glBindTexture(GL_TEXTURE_2D, _textureId);
			if(y!= top-50 && y < top-50)
			{
				
				glColor3f(1.0f, 1.0f, 1.0f);
				glBegin(GL_QUADS);
		
				
				glTexCoord2f(0, 0);
				glVertex2i(x,y); //trai duoi
				glTexCoord2f(1, 0);
				glVertex2i(x,y+25);
				glTexCoord2f(1, 1);//trai tren
				glVertex2i(x+25,y+25); 
				glTexCoord2f(0, 1);//phai tren
				glVertex2i(x+25,y); //
				glEnd();
				glutSwapBuffers();
			}
			if(score==flag)
				return;
				if(flag-1)
				{

					if(flag==-2)//ve chu gameover
					{
						glColor3f(0.0f,10.0f,0.0f);
						setOrthographicProjection();
						glLoadIdentity();
						renderBitmapString(350,500,(void *)font,"GAMEOVER");
					}
					
					if(flag!=-1&&flag!=score)//ve diem, cap do choi, next
					{
						itoa(flag, s, 10);
						glColor3f(0.0f,0.0f,0.0f);
						setOrthographicProjection();
						glLoadIdentity();
						renderBitmapString(700,700,(void *)font,"SCORE");
						renderBitmapString(720,650,(void *)font,s);
						itoa(level-1, s1, 10);
						renderBitmapString(700,600,(void *)font,"LEVEL");
						renderBitmapString(720,550,(void *)font,s1);
						renderBitmapString(700,500,(void *)font,"NEXT BLOCK");
					}
					itoa(score, s, 10);
					glColor3f(10.0f,10.0f,5.0f);
					setOrthographicProjection();
					glLoadIdentity();
					renderBitmapString(700,700,(void *)font,"SCORE");
					renderBitmapString(720,650,(void *)font,s);
					itoa(level, s1, 10);
					renderBitmapString(700,600,(void *)font,"LEVEL");
					renderBitmapString(720,550,(void *)font,s1);
					renderBitmapString(700,500,(void *)font,"NEXT BLOCK");
					flag=score;
				}
		}
		void VienGach::nodisplay()
		{
			if(y!=top-50)
			{
				glBegin(GL_QUADS);
				glColor3f(0.0f, 0.0f, 0.0f);
				glVertex2i(x,y); //trai duoi
				glVertex2i(x,y+25);
				glVertex2i(x+25,y+25); 
				glVertex2i(x+25,y); //
				glEnd();
				glutSwapBuffers();
			}
		}
		void VienGach::Khoa(int board[][maxj])
		{
			block=1;
			board[(y-bottom+50)/25][(x-left)/25]=1;
		}
**Game_Titris.cpp**
// Game_Titris.cpp : Defines the entry point for the console application.
//

#include "stdafx.h"
#include 
#include 
int board[maxi][maxj];
void reset()//gan bang bang 0 
{
	for(int i=0;ifor(int j=0;j0;
}

int Loai(int x)
{
	switch(x)
	{
		case 0:
		return 15;//hinh thang
		break;
		case 1:
		return 31;//hinh vuong
		break;
		case 2:
		return 51;//hinh Z
		break;
		case 3:
		return 30;//hinh Z nguoc
		break;
		case 4:
		return 58;//hinh t
		break;
		case 5:
		return 57;//hinh chu L  
		break;
		case 6:
		return 60;//hinh chu L nguoc
		break;
	}
	return 15;
}
float t=0;//thoi gian 
int chon,chonnext;//random loai khoi gach xuat hien va khoi gach sap xuat hien
KhoiGach *n;//con tro n dung de cap phat khoi gach hien tai
KhoiGach *next;//con tro next dung de cap phat khoi gach tiep theo
GLuint _textureId; //The id of the texture
GLfloat rSquare = 0;
GLuint _textureId1;
GLuint _textureId2;
GLuint _textureId3;
GLuint _textureId4;
GLuint _textureId5;
int score=0;
int level=1;
int flag=-1;
int thoigian=1000;

void processSpecialKeys(int key, int x, int y)//xu ly phim
{
	
	switch(key)
	{
		case GLUT_KEY_LEFT : 
			n->noshow();
			n->dichtrai(board);
			n->show(_textureId,score,level,flag);
				break;
		case GLUT_KEY_UP:	
			
				n->noshow();
				n->xoaykhoi();
				PlaySound(L"down.wav", NULL,  SND_ASYNC);
				PlaySound(L"2.wav", NULL,SND_ASYNC );
				n->show(_textureId,score,level,flag);
			break;//xoay
		case GLUT_KEY_RIGHT:
			n->noshow();
			n->dichphai(board);
			n->show(_textureId,score,level,flag);		
			break;//dich phai
		case GLUT_KEY_DOWN:	
			n->noshow();
			n->dichxuong(board);
			n->show(_textureId,score,level,flag);
			break;//xuong nhanh
	}
}
void background();
void chuotdichuyen(int x,int y)
{
	if(x<(6*25)+left)
	{
			n->noshow();
			n->dichtrai(board);
			n->show(_textureId,score,level,flag);
	}
	else
	{
		n->noshow();
			n->dichphai(board);
			n->show(_textureId,score,level,flag);	
	}
}
void xulychuot(int button,int status,int x,int y)
{
	if(button==1&&status==0)
	{
		n->noshow();
				n->xoaykhoi();
				PlaySound(L"down.wav", NULL,  SND_ASYNC);
				PlaySound(L"2.wav", NULL,SND_ASYNC );
				n->show(_textureId,score,level,flag);
	}
	//lay toa do sau cung,nhap vao mang va xuat ra
	if(button==0&&status==1)
	{
			if(x<(6*25)+left)
		{
				n->noshow();
				n->dichtrai(board);
				n->show(_textureId,score,level,flag);
		}
		else
		{
			n->noshow();
				n->dichphai(board);
				n->show(_textureId,score,level,flag);	
		}
	}

}
void time()
{
		if(flag==-2)
			{
				 //background();
				 return;
			}
		
		t+=0.0001;
		if(t>(thoigian-(level*100)))
		{
			glutSpecialFunc(processSpecialKeys);
			glutMotionFunc(chuotdichuyen);
			glutMouseFunc(xulychuot);
			n->show(_textureId,score,level,flag);
			n->noshow();
			n->KiemTra(board,_textureId, score, level,flag);
			if(flag==-2)
			{
				 background();

				 return;
			}
			if(n->dichxuong(board)==0)
			{
				
				n->codinhkhoi(board);
				n->show(_textureId,score,level,flag);
				delete n;
				n=new KhoiGach(chonnext,0);
				int r=rand()%7;
				chonnext=Loai(r);
				next->noshow();
				next=new KhoiGach(chonnext,1);
				next->show(_textureId,score,level,flag);
			}
				n->show(_textureId,score,level,flag);
			
			t=0;
		}
	
		
}
void  background1()
{
	glBindTexture(GL_TEXTURE_2D, _textureId3);
			
				glColor3f(1.0f, 1.0f, 1.0f);
				glBegin(GL_QUADS);
		
				
				glTexCoord2f(0, 0);
				glVertex2i(0,0); //trai duoi

				
				glTexCoord2f(0, 1);//phai tren
				glVertex2i(0,screenHeight);

				glTexCoord2f(1, 1);//trai tren
				glVertex2i(screenWidth,screenHeight); 

				glTexCoord2f(1, 0);
				glVertex2i(screenWidth,0); //

			
				glColor3f(0.0f, 0.0f, 0.0f);
				glBegin(GL_QUADS);
				glVertex2i(700,670); //trai duoi
				glVertex2i(700,640);
				glVertex2i(760,640); 
				glVertex2i(760,670); //
				glBegin(GL_QUADS);
				glVertex2i(700,570); //trai duoi
				glVertex2i(700,540);
				glVertex2i(760,540); 
				glVertex2i(760,570); //
for(int i=left+25;i<=right-25;i+=25)
		for(int j=bottom;j<=top-50;j+=25)
		{
			//if(i==left+25||i==right-25||j==bottom||j==top-50)
			{
				glColor3f(0.0f, 0.0f, 0.0f);
				glBegin(GL_QUADS);
				glTexCoord2f(0, 0);
				glVertex2i(i,j); //trai duoi
				glTexCoord2f(1, 0);
				glVertex2i(i,j+25);
				glTexCoord2f(1, 1);
				glVertex2i(i+25,j+25);
				glTexCoord2f(0, 1);
				glVertex2i(i+25,j); //
			}
			
		}
		
	for(int i=0;i<5;i++)
		for(int j=0;j<5;j++)
		{
			
			{
				glColor3f(0.0f, 0.0f, 0.0f);
				glBegin(GL_QUADS);
				glTexCoord2f(0, 0);
				glVertex2i((j+5)*25+right,(i+13)*25+bottom); //trai duoi
				glTexCoord2f(1, 0);
				glVertex2i((j+5)*25+right,(i+13)*25+bottom+25);
				glTexCoord2f(1, 1);
				glVertex2i((j+5)*25+right+25,(i+13)*25+bottom+25);
				glTexCoord2f(0, 1);
				glVertex2i((j+5)*25+right+25,(i+13)*25+bottom); //
			}
			
		}
		glEnd();
		glutSwapBuffers();
}
void drawboard()
{
	reset();
	 background1();
	glBindTexture(GL_TEXTURE_2D, _textureId1);
	//glClear(GL_COLOR_BUFFER_BIT);
	for(int i=left+25;i<=right-25;i+=25)
		for(int j=bottom;j<=top-50;j+=25)
		{
			if(i==left+25||i==right-25||j==bottom||j==top-50)
			{
				glColor3f(1.0f, 1.0f, 1.0f);
				glBegin(GL_QUADS);
				glTexCoord2f(0, 0);
				glVertex2i(i,j); //trai duoi
				glTexCoord2f(1, 0);
				glVertex2i(i,j+25);
				glTexCoord2f(1, 1);
				glVertex2i(i+25,j+25);
				glTexCoord2f(0, 1);
				glVertex2i(i+25,j); //
			}
			
		}
		glEnd();
			glutSwapBuffers();
	for(int i=0;i<=22;i++)
	{
		for(int j=0;j<=13;j++)
		{ 
			if(i==2||j==0||j==13)
				board[i][j]=1;
			//printf("%d",board[i][j]);
		}
		//printf("\n");
	}
	
		srand((unsigned int)time(NULL));
		int r=rand()%7;
		chon=Loai(r);
		n=new KhoiGach(chon,0);
		//next
		r=rand()%7;
		chonnext=Loai(r);
		next=new KhoiGach(chonnext,1);
		next->noshow();
		next->show(_textureId,score,level,flag);
		glutIdleFunc(time);
	
	
}
//load hinh
//Makes the image into a texture, and returns the id of the texture
GLuint loadTexture(Image* image)
{
	GLuint textureId;
	glGenTextures(1, &textureId); //Make room for our texture
	glBindTexture(GL_TEXTURE_2D, textureId); //Tell OpenGL which texture to edit
	//Map the image to the texture
	glTexImage2D(GL_TEXTURE_2D,                //Always GL_TEXTURE_2D
				 0,                            //0 for now
				 GL_RGB,                       //Format OpenGL uses for image
				 image->width, image->height,  //Width and height
				 0,                            //The border of the image
				 GL_RGB, //GL_RGB, because pixels are stored in RGB format
				 GL_UNSIGNED_BYTE, //GL_UNSIGNED_BYTE, because pixels are stored
				                   //as unsigned numbers
				 image->pixels);               //The actual pixel data
	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_LINEAR);
	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR);
	return textureId; //Returns the id of the texture
}




void handleResize(int w, int h)
{
	glViewport(0, 0, w, h);
	glClear(GL_COLOR_BUFFER_BIT);
	gluOrtho2D(0.0,screenWidth,0.0,screenHeight);
}

void initRendering()
{

	glEnable(GL_TEXTURE_2D);
	
	Image* image = loadBMP("6.bmp");
	_textureId = loadTexture(image);
	Image* image1 = loadBMP("wall1.bmp");//tuong
	_textureId1 = loadTexture(image1);

	Image* image2 = loadBMP("logo.bmp");//tuong
	_textureId2 = loadTexture(image2);

	Image* image3 = loadBMP("hinhnen.bmp");//tuong
	_textureId3 = loadTexture(image3);
	Image* image4 = loadBMP("phien.bmp");//
	_textureId4= loadTexture(image4);

	Image* image5 = loadBMP("danh.bmp");//
	_textureId5= loadTexture(image5);
	delete image;
	delete image1;
	delete image2;
	delete image3;
	delete image4;
	delete image5;
}
void p_0865100()
{
			glBindTexture(GL_TEXTURE_2D, _textureId4);
			
				glColor3f(1.0f, 1.0f, 1.0f);
				glBegin(GL_QUADS);
		
				
				glTexCoord2f(0, 0);
				glVertex2i(0,0); //trai duoi

				
				glTexCoord2f(0, 1);//phai tren
				glVertex2i(0,screenHeight);

				glTexCoord2f(1, 1);//trai tren
				glVertex2i(screenWidth,screenHeight); 

				glTexCoord2f(1, 0);
				glVertex2i(screenWidth,0); //

				glEnd();
				glutSwapBuffers();
}
void p_0865025()
{
			glBindTexture(GL_TEXTURE_2D, _textureId5);
			
				glColor3f(1.0f, 1.0f, 1.0f);
				glBegin(GL_QUADS);
		
				
				glTexCoord2f(0, 0);
				glVertex2i(0,0); //trai duoi

				
				glTexCoord2f(0, 1);//phai tren
				glVertex2i(0,screenHeight);

				glTexCoord2f(1, 1);//trai tren
				glVertex2i(screenWidth,screenHeight); 

				glTexCoord2f(1, 0);
				glVertex2i(screenWidth,0); //

				glEnd();
				glutSwapBuffers();
}
void setOrthographicProjection()
{

	
	glMatrixMode(GL_PROJECTION);
	glPushMatrix();
	glLoadIdentity();
	gluOrtho2D(0,screenWidth,0,screenHeight);
	//glMatrixMode(GL_MODELVIEW);
}
void renderBitmapString(float x, float y, void *font,char *string)
{
  
  char *c;
  glRasterPos2f(x, y);
  for (c=string; *c != '\0'; c++)
  {
    glutBitmapCharacter(font, *c);
  }
}
int font=(int)GLUT_BITMAP_TIMES_ROMAN_24;
void Highscore()

{
			char s[30];
			int diem;
			ifstream f("output.txt");
			f>>diem;
			glColor3f(10.0f,0.0f,0.0f);
			setOrthographicProjection();
			glLoadIdentity();
			renderBitmapString(450,700,(void *)font,"HIGHSCORE");
			itoa(diem, s, 10);
			renderBitmapString(470,650,(void *)font,s);
			glEnd();
				glutSwapBuffers();
}
void processMenuEvents(int option) 
{
	int fl=-3;
	switch (option)
	{
		
		case 1:
					glClear(GL_COLOR_BUFFER_BIT);
					score=0;
					level=1;
					flag=-1;
					drawboard();
					//PlaySound(L(101), NULL, SND_RESOURCE | SND_ASYNC);
					PlaySound(L"2.wav", NULL,SND_ASYNC );
			break;
		case 2 :
			glClear(GL_COLOR_BUFFER_BIT);
				flag=-2;
				glBindTexture(GL_TEXTURE_2D, _textureId3);
			
				glColor3f(1.0f, 1.0f, 1.0f);
				glBegin(GL_QUADS);
		
				
				glTexCoord2f(0, 0);
				glVertex2i(0,0); //trai duoi

				
				glTexCoord2f(0, 1);//phai tren
				glVertex2i(0,screenHeight);

				glTexCoord2f(1, 1);//trai tren
				glVertex2i(screenWidth,screenHeight); 

				glTexCoord2f(1, 0);
				glVertex2i(screenWidth,0); //
				glEnd();
				glutSwapBuffers();
				Highscore();
		
				break;
		case 3 :
				level=level+1;
				break;
		case 4:
				exit(4);
				break;
		case 5 :
			glClear(GL_COLOR_BUFFER_BIT);
				p_0865100();
				flag=-2;
				break;

		case 25 :
			glClear(GL_COLOR_BUFFER_BIT);
				p_0865025();
				flag=-2;
				break;
		case 7://am thanh
				PlaySound(NULL, NULL,NULL );
			break;
			case 8://am thanh
				PlaySound(L"2.wav", NULL,SND_ASYNC );
			break;
				case 11:
					//background1();
				level=1;
				flag=0;
			break;
				case 12:
				level=2;
				flag=1;
			break;
				case 13:
				level=3;
				flag=2;
			break;	
				case 14:
				level=4;
				flag=3;
			break;
			case 15:
				level=5;
				flag=4;
			break;
				case 16:
				level=6;
				flag=5;
			break;
				case 17:
				level=7;
				flag=6;
			break;
				case 18:
				level=8;
				flag=7;
			break;	
				case 19:
				level=9;
				flag=8;
			break;
	}
}
void createGLUTMenus()
{

	int menu,submenu1,subamthanh,sublevel;
	subamthanh = glutCreateMenu(processMenuEvents);
	glutAddMenuEntry("Off",7);
	glutAddMenuEntry("On",8);
	
	sublevel = glutCreateMenu(processMenuEvents);
	glutAddMenuEntry("1",11);
	glutAddMenuEntry("2",12);
	glutAddMenuEntry("3",13);
	glutAddMenuEntry("4",14);
	glutAddMenuEntry("5",15);
	glutAddMenuEntry("6",16);
	glutAddMenuEntry("7",17);
	glutAddMenuEntry("8",18);
	glutAddMenuEntry("9",19);
	submenu1 = glutCreateMenu(processMenuEvents);
	glutAddSubMenu("Level",sublevel);
	
	glutAddSubMenu("Sound",subamthanh);

	menu = glutCreateMenu(processMenuEvents);
	glutAddMenuEntry("New game",1);
	glutAddMenuEntry("High scores",2);
	glutAddSubMenu("Options",submenu1);
	glutAddMenuEntry("0865100",5);
	glutAddMenuEntry("0865025",25);
	glutAddMenuEntry("Quit game",4);
	glutAttachMenu(GLUT_RIGHT_BUTTON);
}
void background()
{
			glBindTexture(GL_TEXTURE_2D, _textureId2);
			
				glColor3f(1.0f, 1.0f, 1.0f);
				glBegin(GL_QUADS);
		
				
				glTexCoord2f(0, 0);
				glVertex2i(0,0); //trai duoi

				
				glTexCoord2f(0, 1);//phai tren
				glVertex2i(0,screenHeight);

				glTexCoord2f(1, 1);//trai tren
				glVertex2i(screenWidth,screenHeight); 

				glTexCoord2f(1, 0);
				glVertex2i(screenWidth,0); //

				glEnd();
				glutSwapBuffers();
}
int _tmain(int argc, char* argv[])
{
	glutInit(&argc,argv);
	glutInitDisplayMode(GLUT_DOUBLE);
	glutInitWindowSize(screenWidth,screenHeight);
	glutCreateWindow("Game Titris");
	glutInitDisplayMode(GLUT_SINGLE | GLUT_RGB | GLUT_DEPTH);
	initRendering();
	glutDisplayFunc(background);
	createGLUTMenus();
	glutReshapeFunc(handleResize);
	glutMainLoop();
	return 0; 
}
