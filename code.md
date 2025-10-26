#include <opencv2/opencv.hpp>

using namespace cv;
using namespace std;

int main() {
    VideoCapture cap("TrafficLight.mp4");
    VideoWriter writer("output.avi", 0, 30, Size(640, 480));
    
    Mat frame, hsv, red_mask, green_mask;
    vector<vector<Point>> contours;
    
    while (true) {
        cap >> frame;
        if (frame.empty()) break;
        
        cvtColor(frame, hsv, COLOR_BGR2HSV);
        
        inRange(hsv, Scalar(0, 100, 100), Scalar(10, 255, 255), red_mask);
        inRange(hsv, Scalar(40, 50, 50), Scalar(90, 255, 255), green_mask);
        
        findContours(red_mask, contours, RETR_EXTERNAL, CHAIN_APPROX_SIMPLE);
        for (int i = 0; i < contours.size(); i++) {
            vector<Point> approx;
            approxPolyDP(contours[i], approx, 0.02 * arcLength(contours[i], true), true);
            if (approx.size() > 8) {
                Rect rect = boundingRect(contours[i]);
                putText(frame, "Red", Point(rect.x, rect.y-10), 
                        FONT_HERSHEY_SIMPLEX, 0.7, Scalar(0, 0, 255), 2);
            }
        }
        
        findContours(green_mask, contours, RETR_EXTERNAL, CHAIN_APPROX_SIMPLE);
        for (int i = 0; i < contours.size(); i++) {
            vector<Point> approx;
            approxPolyDP(contours[i], approx, 0.02 * arcLength(contours[i], true), true);
            if (approx.size() > 8) {
                Rect rect = boundingRect(contours[i]);
                putText(frame, "Green", Point(rect.x, rect.y-10), 
                        FONT_HERSHEY_SIMPLEX, 0.7, Scalar(0, 255, 0), 2);
            }
        }
        
        writer.write(frame);
    }
    
    return 0;
}
