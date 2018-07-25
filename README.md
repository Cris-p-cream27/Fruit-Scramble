import javafx.application.Application;
import javafx.beans.binding.Bindings;
import javafx.beans.property.BooleanProperty;
import javafx.beans.property.DoubleProperty;
import javafx.beans.property.SimpleBooleanProperty;
import javafx.beans.property.SimpleDoubleProperty;
import javafx.animation.Animation;
import javafx.animation.AnimationTimer;
import javafx.animation.Timeline;
import javafx.event.*;
import javafx.geometry.Insets;
import javafx.geometry.Pos;
import javafx.scene.Scene;
import javafx.scene.control.*;
import javafx.scene.input.KeyEvent;
import javafx.scene.layout.*;
import javafx.scene.paint.Color;
import javafx.scene.shape.Circle;
import javafx.scene.text.Font;
import javafx.scene.text.Text;
import javafx.stage.Stage;

import java.util.Random;

public class Main extends Application {

	public Pane pane;

	public Circle c1;
	public Circle c2;
	public Circle c3;
	public Circle c4;

	public Circle Coin1;
	public Circle Coin2;
	public Circle Coin3;
	public Circle Coin4;
	public Circle Coin5;
	public Circle Coin6;

	public Label lbl;
	public Label lbl2;
	public Label lbl3;
	Random rnd;
	int dist;
	int score;
	int time;
	boolean GoldCoin;

	public static void main(String[] args) {
		launch(args);
	}

	public void start(Stage stage) {

		stage.setTitle("Fruit Scramble");
		AnimationTimer t = new MyTimer();
		rnd = new Random();

		dist = 18;
		time = 0;
		score = 0;
		GoldCoin = false;
		

		Color CoinstrokeColor = Color.rgb(255, 8, 0);
		Color CoinColor = Color.rgb(253, 171, 159);
		Color CoinstrokeColor2 = Color.rgb(240, 94, 35);
		Color CoinColor2 = Color.rgb(249, 166, 2);
		Color strokeColor = Color.rgb(0, 78, 56);
		Color snakeBodyColor = Color.rgb(46, 139, 87);

		// Snake Body
		c1 = new Circle(200, 350, 10);
		c1.setStroke(strokeColor);
		c1.setFill(Color.rgb(46, 139, 87));
		c2 = new Circle(200, 368, 10);
		c2.setStroke(strokeColor);
		c2.setFill(snakeBodyColor);
		c3 = new Circle(200, 386, 10);
		c3.setStroke(strokeColor);
		c3.setFill(snakeBodyColor);
		c4 = new Circle(200, 404, 10);
		c4.setStroke(strokeColor);
		c4.setFill(snakeBodyColor);

		// Trojan horse Gold Coin
		Coin1 = new Circle(200, 404, 15);
		Coin1.setStroke(CoinstrokeColor);
		Coin1.setFill(CoinColor);

		Coin2 = new Circle(610, 20, 15);
		Coin2.setStroke(CoinstrokeColor2);
		Coin2.setFill(CoinColor2);

		Coin3 = new Circle(610, 50, 15);
		Coin3.setStroke(CoinstrokeColor2);
		Coin3.setFill(CoinColor2);

		Coin4 = new Circle(610, 150, 15);
		Coin4.setStroke(CoinstrokeColor);
		Coin4.setFill(CoinColor);

		Coin5 = new Circle(610, 280, 15);
		Coin5.setStroke(CoinstrokeColor);
		Coin5.setFill(CoinColor);

		Coin6 = new Circle(610, 20, 15);
		Coin6.setStroke(CoinstrokeColor2);
		Coin6.setFill(CoinColor2);

		lbl = new Label("Score = 0");
		lbl2 = new Label("Collect at least 500 Points worth of fruit coins to win.");
		lbl3 = new Label(" Watch out some fruit coins are fraudulent, and they will cost you." 
				+ " Good luck baby caterpillar :-)");
		
		 	Label label = new Label();
	        DoubleProperty time = new SimpleDoubleProperty();
	        label.textProperty().bind(time.asString("%.3f seconds"));

	        BooleanProperty running = new SimpleBooleanProperty();

	        AnimationTimer timer = new AnimationTimer() {

	            private long startTime ;
	            
	               @Override
	            public void start() {
	            	
	                startTime = System.currentTimeMillis();
	                running.set(true);
	                super.start();
	            }

	            @Override
	            public void stop() {
	            
	                running.set(false);
	                super.stop();
	            }

	            @Override
	            public void handle(long timestamp) {
	                long now = System.currentTimeMillis();
	                
	                time.set((now - startTime) / 1000.0);
	          
	            }
	        };

//	        public void Winner() {
	//    		
	  //  		if (score > 50) {
	    //			System.out.println("Collect at least 500 Points worth of fruit coins to win."
	    	//				+ " Watch out some fruit coins are fraudulent, and they will cost you." 
	    		//			+ " Good luck baby caterpillar :-)");}
	    			
	//    	}
		
		
		// *****************************************************
		// Create buttons
		// *****************************************************

	        Button startStop = new Button();

	        startStop.textProperty().bind(
	            Bindings.when(running)
	                .then("Reset")
	                .otherwise("Start"));

	        startStop.setOnAction(e -> {
	            if (running.get()) {
	                timer.stop();
	                c1.setCenterX(200);
	    			c1.setCenterY(350);
	    			c2.setCenterX(200);
	    			c2.setCenterY(368);
	    			c3.setCenterX(200);
	    			c3.setCenterY(386);
	    			c4.setCenterX(200);
	    			c4.setCenterY(404);
	    			
	    			GoldCoin = false;
	    			score = 0;
	    			lbl.setText("score = 0");
	    			GetGoldCoin();
	    			
	            } else {
	                timer.start();
	                t.start();
	                c1.setCenterX(200);
	    			c1.setCenterY(350);
	    			c2.setCenterX(200);
	    			c2.setCenterY(368);
	    			c3.setCenterX(200);
	    			c3.setCenterY(386);
	    			c4.setCenterX(200);
	    			c4.setCenterY(404);
	    			
	    			GoldCoin = false;
	    			score = 0;
	    			lbl.setText("score = 0");
	    			GetGoldCoin();
	            } 
	  
	        });
			
	     


		// *****************************************************
		// Create pane, hbox, vbox, scene
		// *****************************************************
		pane = new Pane();
		pane.setMinSize(500, 500);
		GetGoldCoin();
		HBox HB = new HBox();
		HB.setPadding(new Insets(10, 10, 10, 10));
		HB.setSpacing(7);
		HB.getChildren().addAll(startStop);
		VBox VB = new VBox();
		VB.setPadding(new Insets(10, 10, 10, 10));
		VB.setSpacing(7);
		VB.getChildren().addAll(pane, lbl2, lbl3, HB, lbl, label);
		VB.setAlignment(Pos.CENTER);
		Scene scene = new Scene(VB);
		stage.setWidth(660);
		stage.setHeight(700);
		stage.setScene(scene);
		stage.show();
		// *****************************************************
		// Processing Key events
		// *****************************************************
		scene.setOnKeyPressed(new EventHandler<KeyEvent>() {
			@Override
			public void handle(KeyEvent event) {

				switch (event.getCode()) {

				case UP:
					connectSnakeBody();
					c1.setCenterY(c1.getCenterY() - 18);
					break;

				case DOWN:
					connectSnakeBody();
					c1.setCenterY(c1.getCenterY() + 18);
					break;

				case RIGHT:
					connectSnakeBody();
					c1.setCenterX(c1.getCenterX() + 18);
					break;

				case LEFT:
					connectSnakeBody();
					c1.setCenterX(c1.getCenterX() - 18);
					break;

				default: // No default
					break;

				} // end switch

				checkProximity();
			} // end handle

		}); // end OnkeyPressed
	} // end start
		// *****************************************************
		// Membership Functions
		// *****************************************************

	public void GetGoldCoin() { //
		pane.getChildren().clear();
		pane.getChildren().addAll(c4, c3, c2, c1);
	}

	public void connectSnakeBody() { // how the program connects the snake
		c4.setCenterY(c3.getCenterY());
		c4.setCenterX(c3.getCenterX());
		c3.setCenterY(c2.getCenterY());
		c3.setCenterX(c2.getCenterX());
		c2.setCenterY(c1.getCenterY());
		c2.setCenterX(c1.getCenterX());
	}
	
	public void Winner() {
		
		if (score > 50) {
			System.out.println("Collect at least 500 Points worth of fruit coins to win."
					+ " Watch out some fruit coins are fraudulent, and they will cost you." 
					+ " Good luck baby caterpillar :-)");}
			
	}

	public void checkProximity() {

		int GotGoldCoinX = (int) (c1.getCenterX() - Coin1.getCenterX());
		int GotGoldCoinY = (int) (c1.getCenterY() - Coin1.getCenterY());

		int GotGoldCoin2X = (int) (c1.getCenterX() - Coin2.getCenterX());
		int GotGoldCoin2Y = (int) (c1.getCenterY() - Coin2.getCenterY());

		int GotGoldCoin3X = (int) (c1.getCenterX() - Coin3.getCenterX());
		int GotGoldCoin3Y = (int) (c1.getCenterY() - Coin3.getCenterY());

		int DistructoBallX = (int) (c1.getCenterX() - Coin4.getCenterX());
		int DistructoBallY = (int) (c1.getCenterY() - Coin4.getCenterY());

		int DistructoBall2X = (int) (c1.getCenterX() - Coin5.getCenterX());
		int DistructoBall2Y = (int) (c1.getCenterY() - Coin5.getCenterY());

		int DistructoBall3X = (int) (c1.getCenterX() - Coin6.getCenterX());
		int DistructoBall3Y = (int) (c1.getCenterY() - Coin6.getCenterY());

		GotGoldCoinX = Math.abs(GotGoldCoinX);
		GotGoldCoinY = Math.abs(GotGoldCoinY);

		GotGoldCoin2X = Math.abs(GotGoldCoin2X);
		GotGoldCoin2Y = Math.abs(GotGoldCoin2Y);

		GotGoldCoin3X = Math.abs(GotGoldCoin3X);
		GotGoldCoin3Y = Math.abs(GotGoldCoin3Y);

		DistructoBallX = Math.abs(DistructoBallX);
		DistructoBallY = Math.abs(DistructoBallY);

		DistructoBall2X = Math.abs(DistructoBall2X);
		DistructoBall2Y = Math.abs(DistructoBall2Y);

		DistructoBall3X = Math.abs(DistructoBall3X);
		DistructoBall3Y = Math.abs(DistructoBall3Y);

		if (GotGoldCoinX <= 10 && GotGoldCoinY <= 10) { // Got the gold coin
			GetGoldCoin();
			score += 50;
			lbl.setText("score = " + score);
			GoldCoin = false;

		} else {

			if (GotGoldCoin2X <= 10 && GotGoldCoin2Y <= 10) {
				GetGoldCoin();
				score += 25;
				lbl.setText("score = " + score);
				GoldCoin = false;

			} else {

				if (GotGoldCoin3X <= 10 && GotGoldCoin3Y <= 10) {
					GetGoldCoin();
					score += 5;
					lbl.setText("score = " + score);
					GoldCoin = false;

				} else {

					if (DistructoBallX <= 10 && DistructoBallY <= 10) {
						GetGoldCoin();
						score -= 35;
						lbl.setText("score = " + score);
						GoldCoin = false;

					} else {

						if (DistructoBall2X <= 10 && DistructoBall2Y <= 10) {
							GetGoldCoin();
							score -= 20;
							lbl.setText("score = " + score);
							GoldCoin = false;

						} else {

							if (DistructoBall3X <= 10 && DistructoBall3Y <= 10) {
								GetGoldCoin();
								score -= 10;
								lbl.setText("score = " + score);
								GoldCoin = false;

							
								
							}
						}
					}
				}
			}
		}

	} // End checkProximity
		// *****************************************************
		// Inner Class MyTimer
		// *****************************************************
	
	//public void CountDown() {
		
	
		
	//}

	public class MyTimer extends AnimationTimer {
		long lastUpdate = 0;

		public void handle() {
		}

		@Override
		public void handle(long now) {
			lastUpdate++;
			if (lastUpdate >= 20) {
				lastUpdate = 0;
				if (GoldCoin == false) {
					Coin1.setCenterX(rnd.nextInt(400));
					Coin1.setCenterY(rnd.nextInt(400));

					Coin2.setCenterX(rnd.nextInt(300));
					Coin2.setCenterY(rnd.nextInt(300));

					Coin3.setCenterX(rnd.nextInt(200));
					Coin3.setCenterY(rnd.nextInt(200));

					Coin4.setCenterX(rnd.nextInt(500));
					Coin4.setCenterY(rnd.nextInt(500));

					Coin5.setCenterX(rnd.nextInt(100));
					Coin5.setCenterY(rnd.nextInt(100));

					Coin6.setCenterX(rnd.nextInt(550));
					Coin6.setCenterY(rnd.nextInt(550));

					pane.getChildren().addAll(Coin1, Coin2, Coin3, Coin4, Coin5, Coin6);

					GoldCoin = true;

				} else {

					int x = rnd.nextInt(10);
					int y = rnd.nextInt(10);

					int a = rnd.nextInt(20);
					int b = rnd.nextInt(20);

					int c = rnd.nextInt(30);
					int d = rnd.nextInt(30);

					int e = rnd.nextInt(40);
					int f = rnd.nextInt(40);

					int g = rnd.nextInt(50);
					int h = rnd.nextInt(50);

					int i = rnd.nextInt(60);
					int j = rnd.nextInt(60);

					if (x < 5) {
						Coin1.setCenterX(Coin1.getCenterX() - 10);

					} else {
						Coin1.setCenterX(Coin1.getCenterX() + 10);
					}

					if (y < 5) {
						Coin1.setCenterY(Coin1.getCenterY() - 10);

					} else {
						Coin1.setCenterY(Coin1.getCenterY() + 10);
					}

					if (a < 5) {
						Coin2.setCenterX(Coin2.getCenterX() - 10);

					} else {
						Coin2.setCenterX(Coin2.getCenterX() + 10);
					}

					if (b < 5) {
						Coin2.setCenterY(Coin2.getCenterY() - 10);

					} else {
						Coin2.setCenterY(Coin2.getCenterY() + 10);
					}

					if (c < 5) {
						Coin3.setCenterX(Coin3.getCenterX() - 10);

					} else {
						Coin3.setCenterX(Coin3.getCenterX() + 10);
					}

					if (d < 5) {
						Coin3.setCenterY(Coin3.getCenterY() - 10);

					} else {
						Coin3.setCenterY(Coin3.getCenterY() + 10);
					}

					if (e < 5) {
						Coin4.setCenterX(Coin4.getCenterX() - 10);

					} else {
						Coin4.setCenterX(Coin4.getCenterX() + 10);
					}
					if (f < 5) {
						Coin4.setCenterY(Coin4.getCenterY() - 10);

					} else {
						Coin4.setCenterY(Coin4.getCenterY() + 10);
					}

					if (g < 5) {
						Coin5.setCenterX(Coin5.getCenterX() - 10);

					} else {
						Coin5.setCenterX(Coin5.getCenterX() + 10);
					}
					if (h < 5) {
						Coin5.setCenterY(Coin5.getCenterY() - 10);

					} else {
						Coin5.setCenterY(Coin5.getCenterY() + 10);
					}

					if (i < 5) {
						Coin6.setCenterX(Coin6.getCenterX() - 10);

					} else {
						Coin6.setCenterX(Coin6.getCenterX() + 10);
					}
					if (j < 5) {
						Coin6.setCenterY(Coin6.getCenterY() - 10);

					} else {
						Coin6.setCenterY(Coin6.getCenterY() + 10);
					}
						}
			checkProximity();
			
			}
		}
	}
}
