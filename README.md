# Noise-Artwork
A P5.js Artwork Created comprising some randomness

I wanted to do something with points coming together to form a circle and then moving in a clockwise direction and then dispersing again. Randomness in the artwork is bound to the speed of the points when they are scattered and when they keep moving towards a new destination at a different point. All the points start in a circle, each of them completes 1 loop of the circle and then are dispersed to move in random directions for 3 more times, after which they come back to the position they left from and start moving in the clockwise direction again. 
Every time each point joins the circular loop, it changes its color as well. 
You can stop the movement by clicking the mouse button, and you can switch it back on by clicking it again.

## P5.js script:

```
let dim = 400;
let points = [];
let points_count = 150;

let center_x = dim / 2;
let center_y = dim / 2;
let circle_radius = 175;

let can_update_position = true;

function setup() {
  
  angleMode(DEGREES);
  
  createCanvas(dim, dim);
  background(255,255,255);
  
  for(let i = 1; i <= points_count; i++){
    let angle = (360 / points_count) * i;
    points.push(new Point(angle, 3));  
  }
}

function draw() {
  background(0, 0, 0, 10 );
  //fill(0)
  
  for(let i = 0; i < points_count; i++){
    points[i].draw();
    if(can_update_position){
      points[i].updatePosition();  
    }
  }
}

function mouseClicked(){
  can_update_position = !can_update_position;
}

class Point{
  constructor(angle, size){
    this.size = size;
    
    this.can_move = false;
    
    this.angle = angle;
    this.x = center_x + circle_radius * cos(this.angle);
    this.y = center_y + circle_radius * sin(this.angle);
    this.move_along_circle = true;
    this.cycles_to_complete = 1;
    this.destination_angle = (this.angle) + (360 * this.cycles_to_complete);
    
    this.destinations_to_reach = 3;
    this.setColor();
  }
  
  setColor(){
    this.my_color_r = random(10, 255);
    this.my_color_g = random(10, 255);
    this.my_color_b = random(10, 255);
  }
  
  updateDestination(toCircle){
    
    this.speed = random(0.15, 0.3);
    
    if(toCircle){
      this.destinationX = center_x + circle_radius * cos(this.angle);
      this.destinationY = center_y + circle_radius * sin(this.angle);
    }
    else{
      this.destinationX = floor(random(0, dim - 10));
      this.destinationY = floor(random(0, dim - 10));      
    }
    
    this.directionX = (this.destinationX - this.x) / dim;
    this.directionY = (this.destinationY - this.y) / dim;

    this.currDistance = 0;
    this.updateDistance();
    
    this.can_move = true;
  }
  
  updatePosition(){
    if(this.move_along_circle){
      
      if(this.angle >= this.destination_angle){
        this.move_along_circle = false;
        this.updateDestination(false);
        return;
      }
      
      this.angle++;
      this.x = center_x + circle_radius * cos(this.angle);
      this.y = center_y + circle_radius * sin(this.angle);
    }
    else{
      if(this.prevDistance < this.currDistance && this.prevDistance != 0){   
        if(!this.can_move){
            return;
        }
        
        this.can_move = false;
        this.destinations_to_reach--;
        
        if(this.destinations_to_reach == 0){
          this.updateDestination(true);
          return;
        }
        else if(this.destinations_to_reach == -1){
          this.setColor();
          this.destinations_to_reach = 3;
          this.destination_angle = (this.angle) + (360 * this.cycles_to_complete);
          this.move_along_circle = true;
          return;
        }
        else{
          this.updateDestination(false);  
        }
       return;
      }
    
      this.x += (this.directionX * deltaTime * this.speed);
      this.y += (this.directionY * deltaTime * this.speed);
      this.updateDistance();
    }
  }
  
  updateDistance(){
    this.prevDistance = this.currDistance;
    this.currDistance = dist(this.x, this.y, this.destinationX, this.destinationY);
  }
  
  draw(){
    fill(this.my_color_r, this.my_color_g, this.my_color_b, 255);
    circle(this.x, this.y, this.size);
  }
}
    
```

## Link to Sketch:
[https://editor.p5js.org/map.gamedev.ubi/sketches/oi6300MFho](https://editor.p5js.org/map.gamedev.ubi/sketches/oi6300MFho)
