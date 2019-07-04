# Visualizador-de-Audio

    import ddf.minim.*;

    Minim minim;
    AudioInput in;

    int spacing = 16;
    int Xspeed=1;
    int border = spacing*6;
    int amplification = 15;
    int y = spacing;
    float ySteps=50;
      float movX;


     void setup() {
    size(800,800);
    background(255);
    strokeWeight(1);
    noFill();

    minim = new Minim(this);
    in = minim.getLineIn();
    }

     void draw() {
    int x = int(map(movX, 0, in.bufferSize(), 0, 80));

    //float frequency = in.mix.get(x)*spacing*amplification;
    movX= movX + Xspeed;

    float freqMix = in.mix.get(int(x));
    float freqLeft = in.left.get(int(x));
    float freqRight = in.right.get(int(x));
    float amplitude = in.mix.level();
    float size = freqMix * spacing * amplification;

    float red = map(freqLeft, -1, 1, 0, 200);
    float green = map(freqRight, -1, 1, 0, 200);
    float blue = map(freqMix, -1, 1, 0, 55);
    float opacity = map(amplitude, 0, 0.4, 20, 100);

    noStroke();
    fill(red, green, blue, opacity);
    ellipse(movX, ySteps, size, size);

      if (movX > width) {
        movX=0;
        ySteps = ySteps + 30;
      }
     }
