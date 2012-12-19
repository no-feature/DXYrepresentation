DXY's perambulation
=================
Le plotter est un outil pour tracer des plans. Il a la particularité de se déplacer à travers le plan de la feuille pour dessiner. Nous envisageons l'architecture comme la mise en relation d'une forme avec un espace et des corps. Pour rendre compte de cela dans notre projet nous pensons intervenir sur le déplacement du plotter dans l'espace de la page. Nous nous interrogeons désormais sur la définition d'un espace (d'une architecture) par la déambulation.

Direction 1.0:

Mettre en place un système simple de tracking pour pouvoir tester assez rapidement l'impression de ces mouvements dans l'espace avec le plotter.

L'idée était d'abord d'utiliser les fonctions de nos smartphone, mais en vue du temps nous allons tester un tracking lumineux avec processing.

Code processing utilisé:

/**
 * Brightness Tracking 
 * by Golan Levin. 
 * 
 * Tracks the brightest pixel in a live video signal. 
 */


import processing.video.*;

Capture video;

void setup() {
  size(640, 480); // Change size to 320 x 240 if too slow at 640 x 480
  // Uses the default video input, see the reference if this causes an error
  video = new Capture(this, width, height, 30);
  noStroke();
  smooth();
      background(255);

}

void draw() {
  if (video.available()) {
    video.read();
    int brightestX = 0; // X-coordinate of the brightest video pixel
    int brightestY = 0; // Y-coordinate of the brightest video pixel
    float brightestValue = 0; // Brightness of the brightest video pixel
    // Search for the brightest pixel: For each row of pixels in the video image and
    // for each pixel in the yth row, compute each pixel's index in the video
    video.loadPixels();
    int index = 0;
    for (int y = 0; y < video.height; y++) {
      for (int x = 0; x < video.width; x++) {
        // Get the color stored in the pixel
        int pixelValue = video.pixels[index];
        // Determine the brightness of the pixel
        float pixelBrightness = brightness(pixelValue);
        // If that value is brighter than any previous, then store the
        // brightness of that pixel, as well as its (x,y) location
        if (pixelBrightness > brightestValue) {
          brightestValue = pixelBrightness;
          brightestY = y;
          brightestX = x;
        }
           
        index++;
      }
    }
    // Draw a large, yellow circle at the brightest pixel
    fill(0);
    ellipse(brightestX, brightestY, 10 , 10);
   
  }
}
