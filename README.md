# UpdatedCode
Updated code including numerous new methods





Drive









New


.
Folder Path
My Drive

BOTBALL 2019

Programs

BotBall normal Programs









Folders and views



My Drive





Computers





Shared with me





Recent





Starred





Trash





Backups



Storage


1.8 GB of 15 GB used
Upgrade storage


Name
Files







- main.txt







@ Current Program.txt







= Light Sensor.txt







= Range Sensor.txt







= TopHat Sensor.txt

- main.txt


Details

Activity
Today
3:52 PM


Lord Reno uploaded an item

- main.txt

No recorded activity before April 3, 2019

















#include <kipr/botball.h>

//Main Functions
void move			(int speed, int msecs);
void turnRight		(int speed, int msecs);
void turnLeft		(int speed, int msecs);
void moveDistance	(int distance, int speed);

//Servo Functions
void ArmController();
int IsClosed = 0;

//Sensor Functions
void lightSensor();
void RangeSensor();
void TopHatSensor();

//The light  sensor makes use of this
int light    = 0;

//The range  sensor makes use of this
int range = 0;
int NotAlwaysCheck = 10;

//The tophat sensor makes use of this
int surface  = 0;

int lmotor   = 2;
int rmotor   = 1;


int main()
{
    //LIGHT COMMAND
    
    shut_down_in(119);
	// Shuts down all motors after 119 seconds (just less than 2 minutes).
    
    //First Run commands.
    
    turnLeft(1200, 575);	//1200 + 550 = 90* turn.
    ao();
    
    msleep(200);
    
    move(-1200, 3600);
    ao();
    
    turnLeft(1200, 550);	//1200 + 550 = 90* turn.
    ao();
    
    move(-1200, 2090);	//35
    ao();
    
    turnLeft(600, 1415);	//1200 + 550 = 90* turn.
    ao();
    
    move(-1200, 7500);
    ao();
    
    turnLeft(600, 600);	//1200 + 550 = 90* turn.
    ao();
    
    move(-1200, 700);
    ao();
    
    turnLeft(600, 750);	//1200 + 550 = 90* turn.
    ao();
    
    move(-1200, 800);
    ao();
    
    turnLeft(600, 250);	//1200 + 550 = 90* turn.
    ao();
    
    move(-1200, 200);
    ao();
    
    turnLeft(600, 250);	//1200 + 550 = 90* turn.
    ao();
    
    move(-1200, 200);
    ao();
    
    turnLeft(600, 250);	//1200 + 550 = 90* turn.
    ao();
    
    move(-1200, 1100);
    ao();
    
    turnRight(600, 100);	//1200 + 550 = 90* turn.
    ao();
    
    move(1200, 100);
    ao();
    
    //INITIAL PROCESS COMPLETE
    
    move(1200, 5000);
    ao();
    
    turnLeft(600, 450);	//1200 + 550 = 90* turn.
    ao();
    
    //Other Commands
    int forever = 0;
    
    while(forever == 0){
        
    }
    
    
    return 0;
}



//Functions Below
void move(speed, msecs){
	mav (lmotor, speed);
    mav (rmotor,((speed/10)*11));
    msleep(msecs);
}

void turnRight(speed,msecs){
	mav (lmotor,speed);
    mav (rmotor,-speed);
    msleep(msecs);
}

void turnLeft(speed,msecs){
	mav (lmotor,-speed);
    mav (rmotor,speed);
    msleep(msecs);
}

void moveDistance(distance, speed) {
    
    clear_motor_position_counter(1);
    clear_motor_position_counter(2);
    
	while (get_motor_position_counter(2) < distance && get_motor_position_counter(1) < distance) {
		//Ratio 10:12
        mav(lmotor, speed);
		mav(rmotor, ((speed/10)*12));
 	}
 	ao();
}

//Sensor Functions

void lightSensor(){
    
    if(analog(0) < 4000){
        printf("Light \n");
        light = 0;
    }
    
    else if(analog(0) > 4000){
        printf("Dark \n");
        light = 1;
    }
}

void RangeSensor(){
    
    if(NotAlwaysCheck == 10){
        
        if(analog(1) <= 2500){
            printf("Close \n");
            range = 1;
        }

        else if(analog(1) > 2500){
            printf("Far \n");
            range = -1;
            NotAlwaysCheck = 0;
        }
    
    }
    
    else{
        NotAlwaysCheck += 1;
    }
    
}

void TopHatSensor(){
    
    if(analog(5) <= 2000){
        printf("White \n");
        surface = 0;
    }
    
    else if(analog(5) > 2000){
        printf("Black \n");
        surface = 1;
    }
}

void ArmController(){
    
    enable_servos();
    
    //Front Servo (Claw) is in Port 2.
    //Other Servo		 is in Port 3.
    
    //Checks if Servo is Closed or Open.
    // 0 = Open		1 = Closed
    if(IsClosed == 0){
        msleep(1100);
        
        set_servo_position(2, 1170);
        msleep(1000);
        
        set_servo_position(3, 1900);
        msleep(1000);
        
        IsClosed = 1;
    }
    
    else{
        
        turnRight(400, 600);
        ao();
        
        set_servo_position(2, 1550);
        msleep(1000);
        
        set_servo_position(3, 1600);
        msleep(1000);
        
        turnLeft(400, 600);
        ao();
        
        set_servo_position(3, 2040);
        msleep(1000);
        
        IsClosed = 0;
    }
    
    disable_servos();
    
}




lightSensor();
        
        //Moves only on Dark
        if(light == 1){
            move(1000, 1000);
            ao();
        }
        
        //Avoids Light
        if(light == 0){
            move(-1000, 1000);
            turnRight(1000, 1000);
            ao();
        }
        
        msleep(1000);
        
        
        
        
        RangeSensor();
        
        //If an object is close (Moves Back)
        if(range == 0){
            move(500, 1000);
            ao();
        }
        
        //If an object is far (Moves Forward)
        if(range == 1){
            move(-500, 1000);
            ao();
        }
        
        msleep(10);
        
        
        
        TopHatSensor();
        
        //If the surface is white (Moves back, and attempts to fix itself.)
        if(surface == 0){
            //The Robot will move back a bit
            move(-300, 1000);
            ao();
            
            //The Robot will keep turning itself over, until it finds the black line
            while(surface == 0){
                TopHatSensor();
                
                turnRight(100, 100);
         	    ao();
            }
            
        }
        
        //If the surface is black (Moves forward)
        if(surface == 1){
            move(300, 1000);
            ao();
        }
        
        msleep(100);
