# bonus_photoshop


// FCAI – Programming 1 – 2022 - Assignment 3 
// Program Name: bonus_photoshop_20210123_20210383.cpp 
// Program Description: photoshop
// Last Modification Date: 21/4/2022 
// Author1 and ID and Group: 20210123 , A 
// Author2 and ID and Group: 20210383 , A 
// Teaching Assistant:

#include <iostream>
#include <fstream>
#include <cstring>
#include <cmath>
#include "bmplib.h"
#include "bmplib.cpp"
using namespace std;
unsigned char image [SIZE][SIZE][RGB];
unsigned char temp [SIZE][SIZE][RGB];
unsigned char image2 [SIZE][SIZE][RGB];
unsigned char image3 [SIZE][SIZE][RGB];
void loadImage ();
void loadImage2 ();
void saveImage ();
void saveImage2 ();
void rotate_image ();
void rotate90();
void invert_image();
void dark_light();
void merge_images();
void filter4();
void filter1();
void filtera();
void edges();
void blur();
void shrink_image ();
void enlarge_image();
bool not_exit=true;
using namespace std;
double avr =0;
int pixel_x=0,pixel_y=0;
 int sobel_x[3][3] =
    { { -1, 0, 1 },
      { -2, 0, 2 },
      { -1, 0, 1 } };

 int sobel_y[3][3] =
    { { -1, -2, -1 },
      { 0,  0,  0 },
      { 1,  2,  1 } };
int main()
{
   loadImage();
   enlarge_image();
   saveImage2();
}
void loadImage (){
    char imageFileName[100];

   // Get image file name
   cout << "Enter the source image file name: ";
   cin >> imageFileName;

   // Add to it .bmp extension and load image
   strcat (imageFileName, ".bmp");
   readRGBBMP(imageFileName, image);
}
void saveImage () {
   char imageFileName[100];

   // Get image target file name
   cout << "Enter the target image file name: ";
   cin >> imageFileName;

   // Add to it .bmp extension and load image
   strcat (imageFileName, ".bmp");
   writeRGBBMP(imageFileName, image);
}
void filter1()
{
    for (int i = 0; i < SIZE;i++){
        for (int j =0; j<SIZE;j++){
 double avr= (image[i][j][0]* 0.299) + (image[i][j][1]* 0.587) + (image[i][j][2]*0.114);
                  if (avr>127){
                        image[i][j][0]=255;
                        image[i][j][1]=255;
                        image[i][j][2]=255;
                  }
            else{
                    image[i][j][0]=0;
                    image[i][j][1]=0;
                    image[i][j][2]=0;
            }
        }
    }


}

//__________________________


void invert_image() {


  for (int i = 0; i < SIZE; i++) {
    for (int j = 0; j< SIZE; j++) {
    	for(int k = 0 ; k < RGB ; k++)
    	{
		
    		image[i][j][k]=(255-image[i][j][k]);
	}
    }
  }
}

//__________________________


void filter4(){
    int option;
    cout<<"Flip (1)horizontally or (2)vertically ?"<<endl;
    cin>>option;
    if (option==1){
        for (int i = 0; i < SIZE / 2; i++)
    {
        for (int j = 0; j < SIZE ; j++)
        {
            for (int k =0; k<3;k++){
            int swapRow = SIZE - 1 - i;

            unsigned char temp = image[i][j][k];
            image[i][j][k] = image[swapRow][j][k];
            image[swapRow][j][k] = temp;

        }}
    }
}
//vertical
else
for (int i = 0; i < SIZE ; i++)
    {
        for (int j = 0; j < SIZE/2 ; j++)
        {for (int k =0; k<3;k++){
            int swapCol = SIZE - 1 - j;

            unsigned char temp = image[i][j][k];
            image[i][j][k] = image[i][swapCol][k];
            image[i][swapCol][k] = temp;

        }}
    }
}
//_________________________


void loadImage2 (){
    char imageFileName[100];

   // Get gray scale image file name
   cout << "Enter the other source image file name: ";
   cin >> imageFileName;

   // Add to it .bmp extension and load image
   strcat (imageFileName, ".bmp");
   readRGBBMP(imageFileName, image2);
}
//________________________


void dark_light()
{
    int x;
    loadImage();

  cout<<"do you want to make the photo: \n"<<"1-lighter\n"<<"2-darker\n";
  cin>>x;

  if (x==1){
  for (int i = 0; i <255; i++) {
    for (int j = 0; j< 256; j++) {
            for (int k =0; k<3;k++){

          image[i][j][k]+=(256-image[i][j][k])/2;


  }
  }}}
  else if (x==2){

    for (int i = 0; i <255; i++) {
    for (int j = 0; j< 256; j++) {
            for (int k =0; k<3;k++){
       image[i][j][k]-=image[i][j][k]/2;

  }


}}
}}

//_______________________________


void rotate90()
{


	 for (int i = 0; i < SIZE; i++)
   {
       for (int j = i ; j< SIZE; j++) 
	    {
    	for(int k = 0 ; k < RGB ; k++)
    	{
		if(i!=j){
		int transpose = image[i][j][k];
    	image[i][j][k]=image[j][i][k];
    	image[j][i][k]=transpose;
        }
    else
	{
		int transpose = image [i][j][k];
		}
        }
      } 
    }
  

  for (int i=0 ; i < SIZE ; i++)
   {
	for(int j=0 ; j < SIZE/2 ; j++)
	{
		for(int k = 0 ; k < RGB ; k++)
		{
			
		int transpose = image[i][j][k];
		image[i][j][k]=image[i][SIZE-j-1][k];
		image[i][SIZE-j-1][k]= transpose;
	    }
    }
   }
}

//______________________________


void rotate_image() {
	int user_choice;
	cout<<"1-rotate by 90 \n2-rotate by 180 \n3-rotate by 270"<<endl;
	cin>>user_choice;
	if(user_choice==1)
	{
	rotate90();
    }
	else if(user_choice==2)
	{
		rotate90();
		rotate90();
	}
	else if (user_choice==3)
	{
		rotate90();
		rotate90();
		rotate90();
	}
	else
	{
		cout<<"invalid choice"<<endl;

	}
}

//_______________________________


void filtera(){
//lower half
cout<<"1-lower half\n2-right half\n3-left half\n4-upper half\n"<<endl;
int choise;
cin>>choise;
if (choise ==1){
for (int i = 0; i <SIZE;i++){
        for (int j =0; (j<SIZE);j++){
            for (int k =0; k<3;k++){
           image[i][j][k]=image[SIZE-1-i][j][k];

        }

    }}}
    if (choise ==2){
// right half
for (int j = 0; j <SIZE/2;j++){
        for (int i =0; i<SIZE;i++){
            for (int k =0; k<3;k++){
           image[i][j][k]=image[i][SIZE-1-j][k];


}}}}
if (choise ==3){
//left half
for (int j = 0; j <SIZE/2;j++){
        for (int i =0; i<SIZE;i++){
            for (int k =0; k<3;k++){
         image[i][SIZE-1-j][k]= image[i][j][k];


}}}}
if (choise ==4){
//upper half
for (int i = 0; i <SIZE;i++){
        for (int j =0; (j<SIZE);j++){
            for (int k =0; k<3;k++){
          image[SIZE-1-i][j][k]= image[i][j][k];

        }

    }}}
}

//________________________________


void edges(){
   filter1();
     blur();
    for (int x = 0; x< SIZE;x++){
        for (int y=0;y<SIZE;y++){
                for (int k =0; k<3;k++){
                pixel_x = (sobel_x[0][0] * image[x][y][k])
                    + (sobel_x[0][1] * image[x][y+1][k])
                    + (sobel_x[0][2] * image[x][y+2][k])
                    + (sobel_x[1][0] * image[x+1][y][k])
                    + (sobel_x[1][1] * image[x+1][y+1][k])
                    + (sobel_x[1][2] * image[x+1][y+2][k])
                    + (sobel_x[2][0] * image[x+2][y][k])
                    + (sobel_x[2][1] * image[x+2][y+1][k])
                    + (sobel_x[2][2] * image[x+2][y+2][k]);

            pixel_y = (sobel_y[0][0] * image[x][y][k])
                    + (sobel_y[0][1] * image[x][y+1][k])
                    + (sobel_y[0][2] * image[x][y+2][k])
                    + (sobel_y[1][0] * image[x+1][y][k])
                    + (sobel_y[1][1] * image[x+1][y+1][k])
                    + (sobel_y[1][2] * image[x+1][y+2][k])
                    + (sobel_y[2][0] * image[x+2][y][k])
                    + (sobel_y[2][1] * image[x+2][y+1][k])
                    + (sobel_y[2][2] * image[x+2][y+2][k]);

 int val = (int)sqrt((pixel_x * pixel_x) + (pixel_y * pixel_y));

            if(val < 0) val = 0;
            if(val > 255) val = 255;

           image[x][y][k] = val;
        }}
    }

}

//___________________________________


void blur(){
    for(int i=0 ; i < SIZE ; i++)
	{
		for(int j=0 ; j < SIZE ; j++)
		{         for (int k =0; k<3;k++){
			int sum = 0;

			sum = image[i-2][j-2][k]+image[i-1][j-2][k]+image[i][j-2][k]+image[i+1][j-2][k]+image[i+2][j-2][k]+image[i-2][j-1][k]+image[i-1][j-1][k]+image[i][j-1][k]+image[i+1][j-1][k]+image[i+2][j-1][k]+image[i-2][j][k]+image[i-1][j][k]+image[i][j][k]+image[i+1][j][k]+image[i+2][j][k]+image[i-2][j+1][k]+image[i-1][j+1][k]+image[i][j+1][k]+image[i+1][j+1][k]+image[i+2][j+1][k]+image[i-2][j+2][k]+image[i-1][j+2][k]+image[i][j+2][k]+image[i+1][j+2][k]+image[i+2][j+2][k];
			image[i][j][k]=sum/25;
		}
		}
}}

//________________________________


void merge_images()

{
    loadImage();
    loadImage2();


    for (int i = 0; i < 255; i++) {
    for (int j = 0; j< 255; j++) {
            for (int k =0; k<3;k++){
            image3[i][j][k]=(image2[i][j][k]+image[i][j][k])/2;
    }}
  }

    for (int i = 0; i < 255; i++) {
    for (int j = 0; j< 255; j++) {
            for (int k =0; k<3;k++){
            image[i][j][k]=image3[i][j][k];
    }
    }}
}


//______________________________

void shrink_image ()
{	

	int user_choice;
	cout<<"1-shrink to 1/2 \n2-shrink to 1/3 \n3-shrink to 1/4"<<endl;
	cin>>user_choice;
	if(user_choice==1)
	{
	for (int i = 0; i < SIZE; i++) 
	 {
        for (int j = 0; j< SIZE; j++) 
        {  	
           for( int k = 0 ; k < RGB ; k++ )
           {
		   
            image2[i/2][j/2][k] =image[i][j][k];
           }
        }
     }
    }
	else if(user_choice==2)
	{
		for (int i = 0; i < SIZE; i++) 
	 {
        for (int j = 0; j< SIZE; j++) 
        {
		  for(int k = 0; k < RGB ; k++)
		  {
				  	
            image2[i/3][j/3][k] =image[i][j][k];
          }
        }
     }
	}
	else if (user_choice==3)
	{
		for (int i = 0; i < SIZE; i++) 
	 {
        for (int j = 0; j< SIZE; j++) 
        {  	
          for(int k = 0; k < RGB ; k++){
		  
            image2[i/4][j/4][k] =image[i][j][k];
          }
        }
      }
     }
	
	else
	{
		cout<<"invalid choice"<<endl;

	}
	 
}

//__________________________________

void saveImage2 () {
   char imageFileName[100];

   // Get gray scale image target file name
   cout << "Enter the target image file name: ";
   cin >> imageFileName;

   // Add to it .bmp extension and load image
   strcat (imageFileName, ".bmp");
   writeRGBBMP(imageFileName, image2);
}

//____________________________


void enlarge_image()
{
  int quarternum;
    cout<<"please enter which quarter you want to enlarge 1 ,2 ,3 or 4 ";
    cin>>quarternum;

    if(quarternum==1)
    {
    for (int i = 0, k=0 ; i < SIZE; i+=2 ,k++)
            {
                for (int j = 0 , z=0; j < SIZE; j+=2,z++)
                {
                	for(int x = 0 , y = 0 ; x < RGB ; x++ ,y++)
                	{
					
                    image2[i][j][x]    =     image[k][z][y] ;
                    image2[i+1][j][x]   =   image[k][z][y] ;
                    image2[i][j+1][x]   =   image[k][z][y] ;
                    image2[i+1][j+1][x] = image[k][z][y] ;
                    
                    }
                }
             }
    }



    else if (quarternum==2){
        for (int i = 0, k=0 ; k < 128; i+=2 ,k++)
            {
                for (int j = 0 , z=128; z < SIZE; j+=2,z++)
                {
                	for(int x = 0 , y = 0 ; x < RGB ; x++ ,y++)
                	{
                		
                    image2[i][j][x] =   image[k][z][y] ;
                    image2[i+1][j][x] = image[k][z][y] ;
                    image2[i][j+1][x] = image[k][z][y] ;
                    image2[i+1][j+1][x] = image[k][z][y] ;
                    }
                }
            }
    }

    else if (quarternum==3)
        for (int i = 0, k=128 ; k < SIZE; i+=2 ,k++)
            {
                for (int j = 0 , z=0; z < 128; j+=2,z++)
                {
                	for(int x = 0 , y = 0 ; x < RGB ; x++ ,y++)
                	{
                		
                    image2[i][j][x] = image[k][z][y] ;
                    image2[i+1][j][x] = image[k][z][y] ;
                    image2[i][j+1][x] = image[k][z][y] ;
                    image2[i+1][j+1][x] = image[k][z][y] ;
                    }
                }
            }


    else if (quarternum==4)
        for (int i = 0, k=128 ; k < SIZE; i+=2 ,k++)
            {
                for (int j = 0 , z=128; z < SIZE; j+=2,z++)
                {
                   for(int x = 0 , y = 0 ; x < RGB ; x++,y++)
                	{
                		
                    image2[i][j][x] = image[k][z][y] ;
                    image2[i+1][j][x] = image[k][z][y] ;
                    image2[i][j+1][x] = image[k][z][y] ;
                    image2[i+1][j+1][x] = image[k][z][y] ;
                    } 
                }
            }
}

//_______________________

   