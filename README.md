# hw1
# Overview
	運用可調整程式，實現Q1暖色系濾鏡
	運用可調整程式，實現Q3電影感色調
# Implementation
	首先讓暗影變得比較青，就是把紅光關掉，而如何分辨暗影?我是判斷每個點的RGB三色加起來是否小於某個值來分辨，再讓R的數值除1.4變小，會選擇1.4是因為這樣不會有overflow的情況發生。
再來加上橙色濾鏡，因為橙色的分析是(R0.5,G0.5,B*0)，但是濾鏡是額外加上的，所有的點都會覆蓋到，因此連藍色的部分也要加。
最後的結果雖然和網頁類似，但是脖子還有背景中某人的黃衣服，出現了藍色色塊，我覺得網頁上應該是有另外加上模糊的效果，才會看起來不那麼明顯。
 

	#include "stdafx.h"
	#include <iostream>
	#include <opencv2\opencv.hpp>
	int main(){
	cv::Mat image;
	// Read the file
	image = cv::imread("../image/people.jpg");   
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
	>三原色光模式
# Disparity result
	
# Reference
1. 橙色濾鏡參考(https://helpx.adobe.com/tw/photoshop/using/color-monochrome-adjustments-using-channels.html)
2. (http://www.csie.ntnu.edu.tw/~u91029/Image.html)
