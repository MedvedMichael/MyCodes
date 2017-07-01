# MyCodes
ArrayList bullets;
float r,g,b;
int bw=50;
boolean menu=true,exit=false;
int modeNumber=1;
boolean line=false;
boolean boolsize=false;
boolean col=false;

void setup()
{
  size(displayWidth,displayHeight);
  background(255);
  bullets=new ArrayList();
  bullets.add(new Bullet(width/2,height/2,r,g,b,bw));
  
  menuButton[0] = new Button(width/2-250,height/2-150,500, 300,255,0,0, "Play",100);
  menuButton[1] = new Button(20,20,200, 100,0,0,255, "Exit",60); 
 
  gameButton[0] = new Button(width - 100,100,100,100,0,0,255,"To Menu",20);
  gameButton[1] = new Button(width - 100,200,100,100,0,0,255,"Clear",20);
  gameButton[2] = new Button(width - 100,300,100,100,0,0,255,"Color",20);
  gameButton[3] = new Button(width - 100,400,100,100,0,0,255,"Mode",20);
  gameButton[4] = new Button(width - 250,100,100,100,0,0,255,"Size",20);
  
  gameSmallButton[0] = new Button(80,100,80,80,0,0,255,"-",20);
  gameSmallButton[1] = new Button(300,100,80,80,0,0,255,"+",20);
  gameSmallButton[2] = new Button(80,250,80,80,255,0,0,"-",20);
  gameSmallButton[3] = new Button(300,250,80,80,255,0,0,"+",20);
  gameSmallButton[4] = new Button(80,400,80,80,0,255,0,"-",20);
  gameSmallButton[5] = new Button(300,400,80,80,0,255,0,"+",20);
  gameSmallButton[6] = new Button(80,550,80,80,0,0,255,"-",20);
  gameSmallButton[7] = new Button(300,550,80,80,0,0,255,"+",20);
  
}
void draw()
{
  background(255);
  if(menu)
  menu();
  if(!menu)  
  gamePlay(); 
  if(exit) 
  exit();
  
}

void mousePressed()
{
  if(!line)
  bullets.add(new Bullet(mouseX,mouseY,r,g,b,bw));
  
}

 class Button
{
  int x;
  int y;
  int w;
  int h;
  
  int r;
  int g;
  int b;
  
  int txs;
  boolean doing;
  String tex;
  Button(int xx,int yy,int ww,int hh,int rr,int gg,int bb,String texx,int txss)
  {
    x=xx;
    y=yy;
    w=ww;
    h=hh;
    r=rr;
    g=gg;
    b=bb;
    tex=texx;
    txs=txss;
  }
  void display()
  {
    fill(r,g,b,100);
    strokeWeight(6);
    stroke(0);
    rect(x,y,w,h);
    strokeWeight(5);
    stroke(r,g,b,150);
    rect(x,y,w,h);
    textAlign(CENTER);
    fill(255-r,255-g,255-b);
    textSize(txs);
    text(tex,x+0.5*w,y+0.5*h+0.3*txs);
  }
  boolean pressed()
  {
    boolean res=false;
    if(doing)
    {
      if(mousePressed && mouseX>=x && mouseX<=x+w && mouseY>=y && mouseY<=y+h)
      {
        res=true;
        doing=false;
      }
      else
      res=false;
    }
    if(!mousePressed)
    doing=true;
    return res;
  }
}
public class Bullet
{
  float x,y;
  float r,g,b;
  int w;
  float speed=0;
  float gravity = 2;
  float life=255;
  
  Bullet (float xx, float yy, float rr, float gg, float bb, int ww)
  {
    x=xx;
    y=yy;
    r=rr;
    g=gg;
    b=bb;
    w=ww;
    
  }
  
  void appear()
  {
    noStroke();
    fill(color(r,g,b),life);
    ellipse(x,y,w,w); 
  }
  void move()
  {
    speed = speed + gravity;
    y=y+speed;
    if(y>height )
    {
      y=height;
      speed=speed*-0.9;   
    }   
   }  
    boolean finish()
    {
      life--;
      if(life<0) return true;
      else return false;    
  }
}

void clearBullets()
{
  for(int i=bullets.size()-1;i>=0;i--)
  bullets.remove(i);
}

Button [] menuButton = new Button[2];
public void menu()
{
 background(255);
 
 menuButton[0].display();
 menuButton[1].display();
 if(menuButton[0].pressed())
 {
 menu=false;
 clearBullets();
 }
 if(menuButton[1].pressed())
 exit = true;
}

Button [] gameButton = new Button[5];
Button [] gameSmallButton = new Button[8];
public void gamePlay()
{
  background(255);
  for(int i=0;i<5;i++)
  gameButton[i].display();
  
  if(gameButton[0].pressed())
  menu=true;
  if(gameButton[1].pressed())
  clearBullets();
  if(gameButton[2].pressed())
  modeNumber+=1;
  if(gameButton[3].pressed())
  line=!line;
  if(gameButton[4].pressed())
  boolsize=!boolsize;
  
  
 
  
 if(boolsize)
 {
  gameSmallButton[0].display();
  gameSmallButton[1].display();
  if(gameSmallButton[0].pressed())
  bw-=10;
  if(gameSmallButton[1].pressed())
  bw+=10;
  fill(0);
  textAlign(CENTER);
  textSize(30);
  text(bw,230,150);
  textSize(50);
  text("Size",230,80);
  }
  
  if(col)
  {
    for(int i=2;i<8;i++)
    gameSmallButton[i].display();
    if(gameSmallButton[2].pressed())
    r-=10;
    if(gameSmallButton[3].pressed())
    r+=10;
    if(gameSmallButton[4].pressed())
    g-=10;
    if(gameSmallButton[5].pressed())
    g+=10;
    if(gameSmallButton[6].pressed())
    b-=10;
    if(gameSmallButton[7].pressed())
    b+=10;
    fill(0);
    textAlign(CENTER);
    textSize(50);
    text("Color",230,230);
    textSize(30);
    text(r,230,300);
    text(g,230,450);
    text(b,230,600);
    
  }
  
  if(modeNumber>3) modeNumber=1;
  
  for(int i=bullets.size()-1;i>=0;i--)
  {
   Bullet ball= (Bullet) bullets.get(i);
   ball.move();
   ball.appear();
   if(ball.finish())
   {
   bullets.remove(i);
   }
  }
  for(int i=0;i<4;i++)
  {
  if(mousePressed && line)
  bullets.add(new Bullet(mouseX,mouseY,r,g,b,bw));
  }
  
  switch(modeNumber)
  {
  case 1: r=0;
          g=0;
          b=0;
          col=false;
          break;
  case 2: col=true;
          break;
  case 3: r=random(0,255);
          g=random(0,255);
          b=random(0,255);
          col=false;
          break;
  }
  
 
}
