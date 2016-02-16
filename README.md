# hw1
# Overview
	運用可調整程式，實現Q1暖色系濾鏡
	運用可調整程式，實現Q3電影感色調
# Implementation
// 

#include "stdafx.h"
#include <iostream>
#include <opencv2\opencv.hpp>
//#include <cv.h>

//using namespace cv;
//using namespace std;

//blue  pixptr[0] 
//green  pixptr[1] 
//red  pixptr[2]

int main(){

	cv::Mat image;
	image = cv::imread("../image/people.jpg");   // Read the file
	uchar *pixptr;
	if (image.empty()){
		std::cout << "圖片不見了QQ";
		system("pause");
		return 0;
	}
	cv::imwrite("../image/source.png", image);
	cv::imshow("source", image);
	uchar temp;
	for (int i = 0; i<image.rows; i++){
		pixptr = image.ptr<uchar>(i);
		for (int j = 0; j<image.cols; j++){
			
			//陰影變青
			if (pixptr[0] + pixptr[1] + pixptr[2]<280)pixptr[2] /=1.40;
			//加上橙色濾鏡
			temp =( pixptr[1] * 0.5 + pixptr[2] * 0.5 );
			pixptr[2] = pixptr[2]+temp/255;
			pixptr[1] = pixptr[1] + temp / 255;
			pixptr[0] = pixptr[0] + temp / 255;
			pixptr += 3;
		}
	}
	cv::imwrite("../image/Result.png", image);
	cv::imshow("Traverse result", image);
	cv::waitKey(0);

	
	return(0);
}
# Theory
# Disparity result
# Reference
