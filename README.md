```import robocode.*;
import java.awt.Color;
import java.lang.Math;
import static robocode.util.Utils.normalRelativeAngleDegrees;
// API help : https://robocode.sourceforge.io/docs/robocode/robocode/Robot.html

/**
 * HeroOfTime - a robot by (your name here)
 */
public class HeroOfTime extends Robot
{
	//double turnGun = 60 + (Math.random() * 120); //random number between 100,300
	//double randMovement = Math.random() * 300;
	/**
	 * run: HeroOfTime's default behavior
	 */
	public void run() {
		// Initialization of the robot should be put here

		// After trying out your robot, try uncommenting the import at the top,
		// and the next line:

		setColors(Color.green,Color.white,Color.red, Color.orange, Color.orange);
		// Robot main loop
		while (true) {
			turnGunRight(22); // Scans automatically
		}
	}
	

	/**
	 * onScannedRobot: What to do when you see another robot
	 */
	public void onScannedRobot(ScannedRobotEvent e) {
			// Calculate exact location of the robot
		//using the degrees 
		//distance 
		double absoluteBearing = getHeading() + e.getBearing();
		//distance scanned robot from our tanks gun
		double bearingFromGun = normalRelativeAngleDegrees(absoluteBearing - getGunHeading());

		// If it's close enough, fire!
		if (Math.abs(bearingFromGun) <= 3) {
			turnGunRight(bearingFromGun);
			// We check gun heat here, because calling fire()
			// uses a turn, which could cause us to lose track
			// of the other robot.
			//gun only fires when gunHeat is 0
			//power of the bullet depends on distance apart from robot
			if (getGunHeat() == 0) {
				if(e.getDistance() > 0 && e.getDistance() <= 125){
					fire(3.8);
				}
				if(e.getDistance() > 125 && e.getDistance() <= 450){
					fire(2);
				}
				if(e.getDistance() > 450 && e.getDistance() <= 675){
					fire(1);
				}
				if(e.getDistance() > 675 && e.getDistance() <= 900){
					fire(0.7);
				} else {
					fire(0.5);
				}
				
				back(15);
			}
		} // otherwise just set the gun to turn.
		// Note:  This will have no effect until we call scan()
		else {
			turnGunRight(bearingFromGun);
		}
		// Generates another scan event if we see a robot.
		// We only need to call this if the gun (and therefore radar)
		// are not turning.  Otherwise, scan is called automatically.
		if (bearingFromGun == 0) {
			scan(); //scan is called everytime your tank does an action (moves, fires, etc)
		}
	}

	/**
	 * onHitByBullet: What to do when you're hit by a bullet
	 */
	public void onHitByBullet(HitByBulletEvent e) {
		// Replace the next line with any behavior you would like
		turnRight(25);
		back(35);
	}
	
	public void onHitRobot(HitRobotEvent e) {
	     if (e.getBearing() > -90 && e.getBearing() <= 90) {
          back(90);
		  turnRight(45);
       } else {
           ahead(90);
		   turnRight(45);
       }
	}
	
	public void onHitWall(HitWallEvent e) {
	       if (e.getBearing() > -90 && e.getBearing() <= 90) {
         turnLeft(180);
       } else {turnLeft(180);}
	}
	   
	public void onWin(WinEvent e) {
		ahead(8);
		turnRight(36000);
	}

}
```
