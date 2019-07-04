# Visualizador-de-Audio

Este ejemplo toma como input un archivo de sonido y ejecuta tal cual como la imagen "Ejemplo de referencia" adjuntado.

    import ddf.minim.*;

    Minim minim;
    AudioPlayer song;

    int spacing=16;
    int border=spacing*2;
    int amplification=3;
    int y=spacing;
    float ySteps;

    void setup() {
      size(800,800);
      background(255);
      //strokeWeight(2);

      //fill(random(255), random(255), random(255),40);

      minim = new Minim(this);
      song=minim.loadFile("tomorrow.mp3");
      song.play();
    }

    void draw() {
      int screenSize = int((width-2*border)*(height-1.5*border)/spacing);
      int x = int(map(song.position(), 0, song.length(), 0, screenSize));
      ySteps = x/(width-2*border);
      x -= (width-2*border)* ySteps;

      float frequency = song.mix.get(int(x))*spacing*amplification;
      float freqMix = song.mix.get(int(x));
      float freqLeft = song.left.get(int(x));
      float freqRight = song.right.get(int(x));

      float amplitude = song.mix.level();
      float size = freqMix * spacing * amplification;

      float red = map(freqLeft, -1, 1, 0, 200); // jugar con primeros 2 valores hacen cambiar
                                                //ligeramente los colores
      float green = map(freqRight, -1, 1, 0, 50);
      float blue = map(freqMix, -1, 2, 0, 200);
      float opacity = map(amplitude, 0, 0.4, 20, 100);

      noStroke();
      fill(red, green, blue, opacity);
      ellipse(x+border, y*ySteps+border, size, size);


      //if (amplitude < 0.1) { // jugar con el de abajo
        if (amplitude < 0.1) {
        noStroke();
      } else {
        stroke(red, green, blue);
      }

      fill(red, green, blue, 15);

      pushMatrix();
      translate (x+border, y*ySteps+border);
      int circleResolution = (int)map(amplitude, 0, 0.6, 3, 8);
      float radius = size/2;
      float angle = TWO_PI/circleResolution;
      beginShape();
      for (int i= 0; i <=circleResolution; i++) {
        float xShape = 0 + cos(angle*i) * radius;
        float yShape = 0 + sin(angle*i) * radius;
        vertex(xShape, yShape);
      }
      endShape();
      popMatrix();



    }


    void stop() {
      song.close();
      minim.stop();
      super.stop();
    }
