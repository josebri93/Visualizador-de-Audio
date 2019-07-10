    import ddf.minim.*;

    Minim minim;
    AudioInput in;

    int spacing = 16;
    int Xspeed=1;
    int border = spacing*6;
    int amplification = 100;
    int y = spacing;
    float ySteps=50;
      float movX;


    void setup() {
      size(800,800);
      background(255);
      //strokeWeight(1);
      //noFill();

      minim = new Minim(this);
      in = minim.getLineIn();
    }

    void draw() {
      int x = int(map(movX, 0, in.bufferSize(), 0, 80));

      //float frequency = in.mix.get(x)*spacing*amplification;


      float freqMix = in.left.get(int(x));
      float freqLeft = in.left.get(int(x));
      float freqRight = in.left.get(int(x));
      float amplitude = in.left.level();
      float size = freqMix*spacing*amplification; //amplitud total

      float red = map(freqLeft, -1, 1, 0, 200);
      float green = map(size, 3, 15, 51, 150);
      float blue = map(freqMix, -1, 2, 0, 55);
      float opacity = map(amplitude, 0, 0.4, 20, 100);

      //noStroke();

      pushMatrix();
      translate(50,50);
      stroke(0,opacity);
      fill(red, green, blue, opacity);
      ellipse(movX, ySteps, size, size);

      println(size);



      if (amplitude < 0.1) {
        noStroke();
      } else {
        strokeWeight(amplitude*10);
        stroke(red, green, blue);
      }
        fill(red, green, blue, opacity);

      if (size > 1) {
        movX= movX + Xspeed; //iniciador de movimiento
      }
        if (movX > 700) {
          movX=0;
          ySteps = ySteps + 30;
        }

      pushMatrix();
      translate(movX, ySteps);
      int circleResolution = (int)map(amplitude, 0, 1, 3, 12);
      float radius = size/2;
      float angle = TWO_PI/ circleResolution;
      beginShape();
      for (int i= 0; i < circleResolution; i++) {
        float xShape = 0 + cos(angle*i) * radius;
        float yShape = 0 + sin(angle*i) * radius;
        vertex(xShape, yShape);
      }
       endShape();
       popMatrix();

       popMatrix();
    }
