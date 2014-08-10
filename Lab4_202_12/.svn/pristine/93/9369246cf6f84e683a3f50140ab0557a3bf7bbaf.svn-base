package ca.uwaterloo.Lab4_202_12;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

import mapper.InterceptPoint;
import mapper.MapLoader;
import mapper.MapView;
import mapper.NavigationalMap;
import mapper.PositionListener;
import mapper.VectorUtils;

import ca.uwaterloo.Lab4_202_12.R;

import android.app.Activity;
import android.graphics.Color;
import android.graphics.PointF;
import android.hardware.Sensor;
import android.hardware.SensorEvent;
import android.hardware.SensorEventListener;
import android.hardware.SensorManager;
import android.os.Bundle;
import android.util.Log;
import android.view.ContextMenu;
import android.view.ContextMenu.ContextMenuInfo;
import android.view.Menu;
import android.view.MenuItem;
import android.view.View;
import android.widget.Button;
import android.widget.ImageView;
import android.widget.LinearLayout;
import android.widget.TextView;

public class MainActivity1 extends Activity {

	static LineGraphView graph;
	public MapView mv; 
	
	
	
	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main_activity1);
		
		mv = new MapView ( getApplicationContext (), 900 , 500 , 40, 40); //implementing map
		registerForContextMenu ( mv );
		
		NavigationalMap map = MapLoader.loadMap ( getExternalFilesDir ( null ), //reading map file
				"Lab-room-peninsula.svg");
		mv.setMap (map);
		
		LinearLayout layout = (LinearLayout) findViewById(R.id.label);
		TextView[] textviews = new TextView[3]; //array of text boxes for easier implementation
		TextView directions= (TextView)findViewById(R.id.tvHeading);
		
		for(int i = 0; i < textviews.length; i++)
		{
			textviews[i] = new TextView (getApplicationContext());
			textviews[i].setTextColor(Color.RED);
		}
		
//		textviews[0].setText();
		
		graph = new LineGraphView(getApplicationContext(), //using linegraph class to plot data
				100,
				Arrays.asList("x", "y", "z"));
		
		layout.addView(mv);
		layout.addView(graph);
		graph.setVisibility(View.VISIBLE);
		
		for (TextView tv : textviews)
			layout.addView(tv);
		layout.setOrientation(LinearLayout.VERTICAL);
		
		PositionHearer positionListener = new PositionHearer(mv);
		mv.addListener(positionListener);
		
		
		
		SensorManager sensorManager = (SensorManager) getSystemService(SENSOR_SERVICE); //use sensor manager to access linear accelerometer

		Sensor linearAccelerometer = sensorManager.getDefaultSensor(Sensor.TYPE_LINEAR_ACCELERATION);
		Sensor accelerometer = sensorManager.getDefaultSensor(Sensor.TYPE_ACCELEROMETER);
		Sensor magneticField = sensorManager.getDefaultSensor(Sensor.TYPE_MAGNETIC_FIELD);
		
		final LinearAccelerometerEventListener eventListener = new LinearAccelerometerEventListener(directions, textviews[2], positionListener, mv);
		//final AccelerometerEventListener eventListener1 = new AccelerometerEventListener();
		//final MagneticFieldEventListener eventListener2 = new MagneticFieldEventListener(textviews[2]);
		
		sensorManager.registerListener(eventListener, linearAccelerometer, SensorManager.SENSOR_DELAY_FASTEST);
		sensorManager.registerListener(eventListener.sm.eventListener2.eventListener1, accelerometer, SensorManager.SENSOR_DELAY_NORMAL);
		sensorManager.registerListener(eventListener.sm.eventListener2, magneticField, SensorManager.SENSOR_DELAY_FASTEST);
		
		final Button button = (Button) findViewById(R.id.button1); //implement clear button
		button.setOnClickListener(new View.OnClickListener() {
	        public void onClick(View v) {
	        	
	        	eventListener.reset(); //calling reset method
	        }
	        	
	        });
		//layout.addView(button);

	}
	
	public void onCreateContextMenu ( ContextMenu menu , View v, ContextMenuInfo menuInfo ) {
		super.onCreateContextMenu (menu , v, menuInfo );
		mv.onCreateContextMenu (menu , v, menuInfo );
	}
	@Override
	public boolean onContextItemSelected ( MenuItem item ) {
		return super . onContextItemSelected ( item ) || mv.onContextItemSelected ( item );
	}

	@Override
	public boolean onCreateOptionsMenu(Menu menu) {
		// Inflate the menu; this adds items to the action bar if it is present.
		getMenuInflater().inflate(R.menu.main_activity1, menu);
		return true;
	}
	
	

}

class Mode {

	int mode;
	PointF testpoint1;
	PointF testpoint2;
	InterceptPoint intersection1;
	List<PointF> pathNodes;
	
	public Mode() {
		mode = 0;
		pathNodes = new ArrayList<PointF>();
	}
	public List<PointF> calculatePath(PointF current, MapView mv) {
		if(current.x == 0f || current.y == 0f || mv.getDestinationPoint().x == 0 || mv.getDestinationPoint().y == 0)
			return null;
		
		pathNodes = new ArrayList<PointF>();
		
		if(pathNodes.isEmpty())
			pathNodes.add(current);
		
		while(!mv.map.calculateIntersections(pathNodes.get(pathNodes.size() - 1), mv.getDestinationPoint()).isEmpty()) {
				
			if(mode == 0) {
				testpoint1 = new PointF(pathNodes.get(pathNodes.size() - 1).x, pathNodes.get(pathNodes.size() - 1).y);
				testpoint2 = new PointF(pathNodes.get(pathNodes.size() - 1).x, pathNodes.get(pathNodes.size() - 1).y);
				intersection1 = mv.map.calculateIntersections(pathNodes.get(pathNodes.size() - 1), mv.getDestinationPoint()).get(0);
				
				do {
					if(mv.map.calculateIntersections(testpoint1, new PointF(testpoint1.x, testpoint1.y + 0.5f)).isEmpty())
						testpoint1.y = testpoint1.y + 0.5f;
					if(mv.map.calculateIntersections(testpoint2, new PointF(testpoint2.x, testpoint2.y - 0.5f)).isEmpty())
						testpoint2.y = testpoint2.y - 0.5f;
					
					if(!mv.map.calculateIntersections(testpoint1, new PointF(testpoint1.x, testpoint1.y + 0.5f)).isEmpty() && 
						!mv.map.calculateIntersections(testpoint2, new PointF(testpoint2.x, testpoint2.y - 0.5f)).isEmpty()) {
						mode = 1;
						break;
					}
					
					if (mv.map.calculateIntersections(testpoint1, mv.getDestinationPoint()).isEmpty()) {
						pathNodes.add(testpoint1);
						pathNodes.add(mv.getDestinationPoint());
						return pathNodes;
					}
					if (mv.map.calculateIntersections(testpoint2, mv.getDestinationPoint()).isEmpty()) {
						pathNodes.add(testpoint2);
						pathNodes.add(mv.getDestinationPoint());
						return pathNodes;
					}
					Log.d("11231231", "1239817249871237918591364");
					
				}
				while(((mv.map.calculateIntersections(testpoint1, mv.getDestinationPoint()).get(0).getPoint().x - intersection1.getPoint().x) > - 0.2f
						&& (mv.map.calculateIntersections(testpoint1, mv.getDestinationPoint()).get(0).getPoint().x - intersection1.getPoint().x) < 0.2f)
						&& ((mv.map.calculateIntersections(testpoint2, mv.getDestinationPoint()).get(0).getPoint().x - intersection1.getPoint().x) > - 0.2f
						&& (mv.map.calculateIntersections(testpoint2, mv.getDestinationPoint()).get(0).getPoint().x - intersection1.getPoint().x) < 0.2f));
				
				if(!((mv.map.calculateIntersections(testpoint1, mv.getDestinationPoint()).get(0).getPoint().x - intersection1.getPoint().x) > - 0.2f
						&& (mv.map.calculateIntersections(testpoint1, mv.getDestinationPoint()).get(0).getPoint().x - intersection1.getPoint().x) < 0.2f)) {
					pathNodes.add(testpoint1);
					mode = 1;
				}
				else if(!((mv.map.calculateIntersections(testpoint2, mv.getDestinationPoint()).get(0).getPoint().x - intersection1.getPoint().x) > - 0.2f
						&& (mv.map.calculateIntersections(testpoint2, mv.getDestinationPoint()).get(0).getPoint().x - intersection1.getPoint().x) < 0.2f)) {
					pathNodes.add(testpoint2);
					mode = 1;
				}
//				else if(!mv.map.calculateIntersections(pathNodes.get(pathNodes.size() - 1), testpoint1).isEmpty() || !mv.map.calculateIntersections(current, testpoint2).isEmpty()) {
//					return pathNodes;
//				}	
				
			}
			
			if(mode == 1) {
				
				testpoint1 = new PointF(pathNodes.get(pathNodes.size() - 1).x, pathNodes.get(pathNodes.size() - 1).y);
				testpoint2 = new PointF(pathNodes.get(pathNodes.size() - 1).x, pathNodes.get(pathNodes.size() - 1).y);
				
				intersection1 = mv.map.calculateIntersections(pathNodes.get(pathNodes.size() - 1), mv.getDestinationPoint()).get(0);
				
				do {
					if(mv.map.calculateIntersections(testpoint1, new PointF(testpoint1.x + 1f, testpoint1.y)).isEmpty())
						testpoint1.x = testpoint1.x + 1f;
					if(mv.map.calculateIntersections(testpoint2, new PointF(testpoint2.x - 1f, testpoint2.y)).isEmpty())
						testpoint2.x = testpoint2.x - 1f;
					
					if(!mv.map.calculateIntersections(testpoint1, new PointF(testpoint1.x + 1f, testpoint1.y)).isEmpty() && 
							!mv.map.calculateIntersections(testpoint2, new PointF(testpoint2.x - 1f, testpoint2.y)).isEmpty()) {
							mode = 0;
							break;
					}
					
					if (mv.map.calculateIntersections(testpoint1, mv.getDestinationPoint()).isEmpty()) {
						pathNodes.add(testpoint1);
						pathNodes.add(mv.getDestinationPoint());
						return pathNodes;
					}
					if (mv.map.calculateIntersections(testpoint2, mv.getDestinationPoint()).isEmpty()) {
						pathNodes.add(testpoint2);
						pathNodes.add(mv.getDestinationPoint());
						return pathNodes;
					}
					
					Log.d("asdkfhalksghlagfasgfa", "asdfkugasluialgawegfawef");
				}
				while (!mv.map.calculateIntersections(pathNodes.get(pathNodes.size() - 1), mv.getDestinationPoint()).isEmpty());
//				while(((mv.map.calculateIntersections(testpoint1, mv.getDestinationPoint()).get(0).getPoint().y - intersection1.getPoint().y) > - 0.2f
//						&& (mv.map.calculateIntersections(testpoint1, mv.getDestinationPoint()).get(0).getPoint().y - intersection1.getPoint().y) < 0.2f)
//						&& ((mv.map.calculateIntersections(testpoint2, mv.getDestinationPoint()).get(0).getPoint().y - intersection1.getPoint().y) > - 0.2f
//						&& (mv.map.calculateIntersections(testpoint2, mv.getDestinationPoint()).get(0).getPoint().y - intersection1.getPoint().y) < 0.2f));
				
				if(!((mv.map.calculateIntersections(testpoint1, mv.getDestinationPoint()).get(0).getPoint().y - intersection1.getPoint().y) > - 0.2f
						&& (mv.map.calculateIntersections(testpoint1, mv.getDestinationPoint()).get(0).getPoint().y - intersection1.getPoint().y) < 0.2f)) {
					pathNodes.add(testpoint1);
					mode = 0;
				}
				else if(!((mv.map.calculateIntersections(testpoint2, mv.getDestinationPoint()).get(0).getPoint().y - intersection1.getPoint().y) > - 0.2f
						&& (mv.map.calculateIntersections(testpoint2, mv.getDestinationPoint()).get(0).getPoint().y - intersection1.getPoint().y) < 0.2f)) {
					pathNodes.add(testpoint2);
					mode = 0;
				}
//				else if(!mv.map.calculateIntersections(pathNodes.get(pathNodes.size() - 1), testpoint1).isEmpty() || !mv.map.calculateIntersections(current, testpoint2).isEmpty()) {
//					return pathNodes;
//				}			
			}
		}
		
//		pathNodes.add(mv.getDestinationPoint());
		return pathNodes;
	}
	
	
	static public void updateMap(MapView mv, List<InterceptPoint> intersections, PointF position, Mode mode) {
		intersections = mv.map.calculateIntersections(mv.getDestinationPoint(), mv.getUserPoint());
		
		if (intersections.isEmpty()) {
			//draw lines
			mode.pathNodes = Arrays.asList(mv.getDestinationPoint(), mv.getUserPoint());
			mv.setUserPath(mode.pathNodes);
		}
		else
			mv.setUserPath(mode.calculatePath(position, mv));
	}
}
class PositionHearer implements PositionListener {
	
	PointF position;
	MapView mv;
	List<InterceptPoint> intersections;
	Mode mode;
	
	PositionHearer(MapView mv) {
		this.position = new PointF(0,0);
		this.mv = mv;
		mode = new Mode();
	}
	@Override
	public void originChanged(MapView source, PointF loc) {
		// TODO Auto-generated method stub
		mv.setUserPoint(loc);
		
		position.x = loc.x;
		position.y = loc.y;

		Mode.updateMap(mv, intersections, this.position, mode);
		
	}

	@Override
	public void destinationChanged(MapView source, PointF dest) {
		// TODO Auto-generated method stub
		Mode.updateMap(mv, intersections, this.position, mode);
		
	}
	
	public void updatePosition(PointF position) {
		mv.setUserPoint(position);
		this.position = position;
		Mode.updateMap(mv, intersections, this.position, mode);
		
	}
	
	
}

class Point { //getting position and then calculate displacement
	
	double x;
	double y;
	
	public Point (double x, double y) {
		this.x = x;
		this.y = y;
	}
	
	public void addVector(double x, double y) {
		this.x += x;
		this.y += y;
	}
	
	public double getDisplacement()
	{
		return Math.sqrt(x*x + y*y);
	}	
	
	public double getX() {
		return this.x;
	}
	
	public void setX(double value) {
		this.x = value;
	}
	public double getY() {
		return this.y;
	}
	public void setY(double value) {
		this.y = value;
	}
	
	@Override
	public String toString() {
		return String.format("Postion (East, North): (%.2f, %.2f)", x, y);
	}
}

class MagneticFieldEventListener implements SensorEventListener {
	float[] rotationMatrix;
	float[] inclinationMatrix;
	float[] orientation;
	TextView output;
	float[] deviations;
	int numSamples;
	float baseline;
	boolean isFirstTimeRun;
	AccelerometerEventListener eventListener1;
	float offset = 0.3f;


	public MagneticFieldEventListener(TextView outputView) {
		output = outputView;
		eventListener1 = new AccelerometerEventListener();
		orientation = new float[3];
		inclinationMatrix = new float[9];
		rotationMatrix = new float[9];
		numSamples = 0;
		deviations = new float[100];
		isFirstTimeRun = false;
							
		
		//Sensor accelerometer = sensorManager.getDefaultSensor(Sensor.TYPE_ACCELEROMETER);
	}
	
	public void onAccuracyChanged(Sensor s, int i) {}
	
	public void onSensorChanged(SensorEvent se) {
		
		SensorManager.getRotationMatrix(rotationMatrix, inclinationMatrix, eventListener1.gravity, se.values);
		SensorManager.getOrientation(rotationMatrix, orientation);		
		
		if (isFirstTimeRun) //
			baseline = orientation [0];
			isFirstTimeRun = false;
		if (numSamples < 100)
		{
			if(orientation[0] - baseline > (float)Math.PI)
				deviations[numSamples] = orientation[0] - baseline - 2*(float)Math.PI;
			else if(orientation[0] - baseline < -(float)Math.PI)
				deviations[numSamples] = orientation[0] - baseline + 2*(float)Math.PI;
			else
				deviations[numSamples] = orientation[0] - baseline;
			numSamples++;
		}
		if (numSamples == 100)
		{
			float sum = 0;
			for(float f : deviations)
				sum += f;
			if (baseline + sum / 100f > (float)Math.PI)
				baseline = baseline + sum / 150f - 2*(float)Math.PI;
			else if (baseline + sum / 100f < -(float)Math.PI)
				baseline = baseline + sum / 100f + 2*(float)Math.PI;
			else
				baseline = baseline + sum / 100f;
			numSamples = 0;
		}
		
		String s = String.format("\nOrientation: (%.2f, %.2f, %.2f)\nAverage Azimuth: (%.2f) radians", 
				orientation[0], orientation[1], orientation[2], baseline);
		
		
		output.setText(s);
		
	}
	
}

class AccelerometerEventListener implements SensorEventListener {
	public float[] gravity;
	
	public AccelerometerEventListener (){
		gravity = new float[3];
	}

	public void onAccuracyChanged(Sensor s, int i) {}
	
	public void onSensorChanged(SensorEvent se) {
		gravity = se.values;
		
	}
	
}

class LinearAccelerometerEventListener implements SensorEventListener {
	TextView output;
	double maxValue0;
	double maxValue1;
	double maxValue2;
	float[] smoothedAccel;
	public StateMachine sm;


	public LinearAccelerometerEventListener(TextView outputView, TextView tv, PositionHearer positionListener, MapView mv){
		output = outputView;
		maxValue0 = 0;
		maxValue1 = 0;
		maxValue2 = 0;
		smoothedAccel = new float[]{0,0,0}; //array for smoothedaccel
		sm = new StateMachine(tv, positionListener, mv); 

	}
	public void onAccuracyChanged(Sensor s, int i) {}
	
	public void reset()
	{
		sm.reset();
	}

	public void onSensorChanged(SensorEvent se) {
			
		float previousAccel1 = smoothedAccel[0];
		float previousAccel2 = smoothedAccel[1];
		float previousAccel3 = smoothedAccel[2];
		
		smoothedAccel[0] += (se.values[0] - smoothedAccel[0]) /65; //implementing smoothedaccel (low pass filter) for x,y,z
		smoothedAccel[1] += (se.values[1] - smoothedAccel[1]) /65;
		smoothedAccel[2] += (se.values[2] - smoothedAccel[2]) /65;
		
		String s = String.format("(%.2f, %.2f, %.2f)", smoothedAccel[0], smoothedAccel[1], smoothedAccel[2]);
		
		if(sm.sw.getElapsedTime() >= 5000){
			sm.changeState(0);
		}
		//String s = String.format("(%.2f, %.2f, %.2f)", se.values[0], se.values[1], se.values[2]);
		if (Math.abs(smoothedAccel[0]) > maxValue0){ //setting max values of smoothedaccel for each x,y,z
			
			maxValue0 = Math.abs(smoothedAccel[0]);
		}
		if (Math.abs(smoothedAccel[1]) > maxValue1){
			
			maxValue1 = Math.abs(smoothedAccel[1]);
		}
		if (Math.abs(smoothedAccel[2]) > maxValue2){
			
			maxValue2 = Math.abs(smoothedAccel[2]);
		}
		String maxValues = String.format("(%.2f, %.2f, %.2f)", maxValue0, maxValue1, maxValue2); //output max values of smoothedaccel on screen
		
		if(smoothedAccel[2] > -0.2 /*&& (smoothedAccel[2] > previousAccel3) */&& (sm.state == 0))
		{
			sm.changeState(1);
		}
		
		else if(smoothedAccel[2] > 0.3 && (sm.state == 1))
		{
			sm.changeState(2);
		}
		
		else if(smoothedAccel[2] < 0.3 /*&& smoothedAccel[2] < previousAccel3 */&& sm.state == 2 )
		{
			sm.changeState(3);
		}
		
		else if(smoothedAccel[2] < -0.2 && (sm.state == 3))
		{
			sm.changeState(0);
		}
		
		String d = String.format("\n\nDisplacement: %.2f", sm.position.length());
		
		String directions = "Walk Forward";
		
		if (sm.positionListener.mode.pathNodes.size() > 0) {
			
			Log.d("soyijdgjnkjnyrsylknlknrylknrsy", "anglebetweeeeeeen");
			float angleBetween = VectorUtils.angleBetween(sm.position, sm.positionListener.mode.pathNodes.get(1),  new PointF((float)(sm.position.x + Math.sin(sm.eventListener2.baseline)), (float)(sm.position.y - Math.cos(sm.eventListener2.baseline))));
			if (VectorUtils.distance(sm.positionListener.mv.getDestinationPoint(), sm.position) < 1) {
				directions = "You have reached your destination.";
			}
		    if (angleBetween < -0.2f) {
				directions = "Turn Right";
			}
			else if (angleBetween > 0.2f) {
				directions = "Turn Left";
			}
		}
		
		output.setText("Directions:\n" + directions + "\n--Steps--\n" + sm.steps + "\n\nAcceleration\nCurrent Value: " + s + "\nRecord Value: " + maxValues +"\nState:" + sm.state + d
				+ "\n" + sm.position);
		
		//if (se.sensor.getType() == (Sensor.TYPE_LINEAR_ACCELERATION))
			MainActivity1.graph.addPoint(smoothedAccel);
			//MainActivity1.graph.addPoint((float[])se.values);
	}
}

class StateMachine //finite statemachine 
{
	public int state;
	public StopWatch sw;
	public int steps;
	long time0;
	long time1;
	long time2;
	public PointF position;    
	public MagneticFieldEventListener eventListener2;
	public PositionHearer positionListener;
	public MapView mv;
	
	public StateMachine(TextView tv, PositionHearer positionListener, MapView mv)
	{
		state = 0;
		sw = new StopWatch();
		steps = 0;
		position = positionListener.position;
		eventListener2 = new MagneticFieldEventListener(tv);
		this.positionListener = positionListener;
		this.mv = mv;
	}
	
	public void changeState(int newState) //changing the state of the finite state machine
	{
		if (newState == state)
		{
			//do nothing
		}
		/*else if(sw.getElapsedTime() > 2000) //using stopwatch class for time reset
		{
			state = 0;
			sw.stop();
			sw.start();
		}*/
		else if (newState == 1) //different cases for different states
		{
			sw.start();
			state = newState;
		}
		else if (newState == 2)
		{
			sw.stop();
			time0 = sw.getElapsedTime();
			sw.start();
			state = newState;
		}
		else if (newState == 3)
		{
			sw.stop();
			time1 = sw.getElapsedTime();
			sw.start();
			state = newState;
		}
		else if (newState == 0)
		{
			sw.stop();
			time2 = sw.getElapsedTime();
			state = newState;
			
			// check parameters time1, time2, time3 and use those times to determine if step should be counted
			if(time0 > 30 && time0 < 1000 && time1 > 30 && time1 < 1000 && time2 > 30 && time2 < 1000) 
			{
				steps++;
				if(mv.map.calculateIntersections(position, new PointF((float)(position.x + 0.85 * Math.sin(eventListener2.baseline)), (float)(position.y - 0.85 * Math.cos(eventListener2.baseline)))).isEmpty()) {
					position.x +=  0.85 * Math.sin(eventListener2.baseline + eventListener2.offset);
					position.y += -0.85 * Math.cos(eventListener2.baseline + eventListener2.offset);
					positionListener.updatePosition(position);
				}
				
			}
		}
	}
	public void reset()
	{
		sw.stop();
		sw.start();
		steps = 0;
		state = 0;
		
	}
}

/*
Copyright (c) 2005, Corey Goldberg

StopWatch.java is free software; you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation; either version 2 of the License, or
(at your option) any later version.
*/

class StopWatch {
    
    private long startTime = 0;
    private long stopTime = 0;
    private boolean running = false;

    
    public void start() {
        this.startTime = System.currentTimeMillis();
        this.running = true;
    }

    
    public void stop() {
        this.stopTime = System.currentTimeMillis();
        this.running = false;
    }

    
    //elapsed time in milliseconds
    public long getElapsedTime() {
        long elapsed;
        if (running) {
             elapsed = (System.currentTimeMillis() - startTime);
        }
        else {
            elapsed = (stopTime - startTime);
        }
        return elapsed;
    }
    
    
    //elapsed time in seconds
    public long getElapsedTimeSecs() {
        long elapsed;
        if (running) {
            elapsed = ((System.currentTimeMillis() - startTime) / 1000);
        }
        else {
            elapsed = ((stopTime - startTime) / 1000);
        }
        return elapsed;
    }
}
