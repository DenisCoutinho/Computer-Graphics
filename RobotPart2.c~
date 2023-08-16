/*******************************************
  Projeto de Computacao Grafica
  Grupo: Guilherme Vieira e Matheus Amoreli  
*******************************************/
#include <stdio.h>
#include <stdlib.h>
#include <math.h>
#include <GL/glut.h>
#include "png_texture.h"

#define PI 3.1415
#define TEXTURA_DO_BRACO "grama2.png"
#define TEXTURA_DO_TEAPOT "madeira1.png"
#define COORD_TEXTURA_PLANO 1.0
#define COORD_TEXTURA_TEAPOT 1.0

GLint WIDTH =860;
GLint HEIGHT=580;

GLuint  textura_plano;
GLuint  textura_teapot;

//Vetores para o observador
GLfloat obs[3]={0.0,6.0,0.0};
GLfloat olho[3]={0.0,3.0,0.0};
//Vetores das coordenadas dos objetos
GLfloat coordEsf[3]={2.0,0.5,3.0};
GLfloat coordBule[3]={3.5,0.75,-1.5};
GLfloat coordCubo[3]={2.0,0.5,-3.0};

GLfloat mouseX = 0.0, mouseY = 0.0;

//Variaveis usadas para pegar e soltar objetos
GLint pegar1 = 0, pegar2 = 0, pegar3 = 0; 

GLfloat ctp[4][2]={
  {-COORD_TEXTURA_PLANO,-COORD_TEXTURA_PLANO},
  {+COORD_TEXTURA_PLANO,-COORD_TEXTURA_PLANO},
  {+COORD_TEXTURA_PLANO,+COORD_TEXTURA_PLANO},
  {-COORD_TEXTURA_PLANO,+COORD_TEXTURA_PLANO}
};

//Luz do plano
GLfloat plano_difusa[]    = { 0.8, 0.8, 1.0, 1.0 };
GLfloat plano_especular[] = { 1.0, 1.0, 1.0, 1.0 };
GLfloat plano_brilho[]    = { 50.0 };
// Cor e Luz do cubo 
GLfloat mat_a_difusa[]    = { 1.0, 0.0, 0.0, 1.0 };
GLfloat mat_a_especular[] = { 0.0, 0.0, 0.0, 1.0 };
GLfloat mat_a_brilho[]    = { 100.0 };
// Cor e Luz da esfera
GLfloat mat_b_difusa[]    = { 0.0, 0.1, 0.9, 1.0 };
GLfloat mat_b_especular[] = { 1.0, 1.0, 1.0, 1.0 };
GLfloat mat_b_brilho[]    = { 50.0 };
//Posicao e cor da fonte de luz
GLfloat posicao_luz0[]    = { 7.0, 10.0, 0.0, 1.0};
GLfloat cor_luz0[]        = { 1.0, 1.0, 1.0, 1.0};
GLfloat cor_luz0_amb[]    = { 0.6, 0.6, 0.6, 1.0};

GLfloat sem_cor[]         = { 0.0, 0.0, 0.0, 1.0};
GLfloat braco_difusa[]    = { 0.8, 0.9, 1.0, 1.0 };  

//Angulos teta  e raios para a movimentacao do OBSERVADOR e dos OBJETOS  
GLfloat tetaxz=-35, raioxz=10;
GLfloat txz1=60, rxz1=4.5;
GLfloat txz=0, rxz=9;
GLfloat txz3=-60, rxz3=4.5;

//-----------------------------Variaveis para controle do braco----------------------------------------
static int shoulder = 0, elbow = 0, baseY = 180, indicator = 0, middle = 0, polegar = 0, mao = 0, bule = 0;


void reshape(int width, int height){
  WIDTH=width;
  HEIGHT=height;
  glViewport(0,0,(GLint)width,(GLint)height);
  glMatrixMode(GL_PROJECTION);
  glLoadIdentity();
  gluPerspective(70.0,width/(float)height,0.1,30.0);
  glMatrixMode(GL_MODELVIEW);
}

void carregar_texturas(void){
  textura_plano = png_texture_load(TEXTURA_DO_BRACO, NULL, NULL);
  textura_teapot= png_texture_load(TEXTURA_DO_TEAPOT, NULL, NULL);
}

void display(void){
  glEnable(GL_DEPTH_TEST);
  glEnable(GL_LIGHTING);
  glEnable(GL_TEXTURE_2D);
  
  glDepthMask(GL_TRUE);
  glClearColor(0.0,0.0,0.0,1.0);
  glClear(GL_COLOR_BUFFER_BIT|GL_DEPTH_BUFFER_BIT);  
  
  glPushMatrix();

  /* calcula a posicao do observador */
  obs[0]=raioxz*cos(2*PI*tetaxz/360);
  obs[2]=raioxz*sin(2*PI*tetaxz/360);
  gluLookAt(obs[0],obs[1],obs[2],olho[0],olho[1],olho[2],0.0,1.0,0.0);
  
  /*posicao dos objetos*/
  coordEsf[0]=rxz1*cos(2*PI*txz1/360);
  coordEsf[2]=rxz1*sin(2*PI*txz1/360);
  coordBule[0]=rxz*cos(2*PI*txz/360);
  coordBule[2]=rxz*sin(2*PI*txz/360);
  coordCubo[0]=rxz3*cos(2*PI*txz3/360);
  coordCubo[2]=rxz3*sin(2*PI*txz3/360);
  
  /* propriedades do material do plano */
  glMaterialfv(GL_FRONT_AND_BACK, GL_DIFFUSE, plano_difusa);
  glMaterialfv(GL_FRONT_AND_BACK, GL_SPECULAR, plano_especular);
  glMaterialfv(GL_FRONT_AND_BACK, GL_SHININESS, plano_brilho);
  
  /* desenha o plano */
  glNormal3f(0,1,0); 
  glPolygonMode( GL_FRONT_AND_BACK, GL_FILL );
  glBindTexture(GL_TEXTURE_2D,textura_plano);
  glBegin(GL_QUADS);
  glTexCoord2fv(ctp[0]);  glVertex3f(-10,0,10);
  glTexCoord2fv(ctp[1]);  glVertex3f(10,0,10);
  glTexCoord2fv(ctp[2]);  glVertex3f(10,0,-10);
  glTexCoord2fv(ctp[3]);  glVertex3f(-10,0,-10);
  glEnd();

  glDisable(GL_TEXTURE_2D);
  
  //Esfera da fonte de luz
  glPushMatrix();
  glTranslatef(posicao_luz0[0],posicao_luz0[1],posicao_luz0[2]);
  glColor3f(1,0,0);
  glMaterialfv(GL_FRONT, GL_EMISSION, cor_luz0);
  glutSolidSphere(0.2,40,40);
  glPopMatrix();
  
  //Teapot
  glEnable(GL_TEXTURE_2D);
  glPushMatrix();
  glScalef(0.5,0.5,0.5);
  glTranslatef(coordBule[0], coordBule[1], coordBule[2]);
  glBindTexture(GL_TEXTURE_2D, textura_teapot);
  glutSolidTeapot(1.0);
  glPopMatrix();
  glDisable(GL_TEXTURE_2D);
  
  //Cubo (Difuso)
  glMaterialfv(GL_FRONT, GL_EMISSION, sem_cor);
  glMaterialfv(GL_FRONT, GL_DIFFUSE, mat_a_difusa);
  glMaterialfv(GL_FRONT, GL_SPECULAR, mat_a_especular);
  glMaterialfv(GL_FRONT, GL_SHININESS, mat_a_brilho);

  glPushMatrix();
  glTranslatef(coordCubo[0], coordCubo[1], coordCubo[2]);
  glutSolidCube(0.7);
  glPopMatrix();
  
  //Esfera (Especular)
  glMaterialfv(GL_FRONT, GL_DIFFUSE, mat_b_difusa);
  glMaterialfv(GL_FRONT, GL_SPECULAR, mat_b_especular);
  glMaterialfv(GL_FRONT, GL_SHININESS, mat_b_brilho);
  
  glPushMatrix();
  glTranslatef(coordEsf[0], coordEsf[1], coordEsf[2]);
  glutSolidSphere(0.5, 40, 40);
  glPopMatrix();
   
   //BRACO ROBOTICO
   //Plataforma
   glMaterialfv(GL_FRONT, GL_EMISSION, sem_cor);
   glMaterialfv(GL_FRONT, GL_DIFFUSE, braco_difusa);
   glMaterialfv(GL_FRONT, GL_SPECULAR, mat_b_especular);
   glMaterialfv(GL_FRONT, GL_SHININESS, mat_a_brilho);
   
   glTranslatef(0.0,0.5,0.0);
   glRotatef ((GLfloat) baseY, 0.0, 1.0, 0.0);
   glPushMatrix();
   glScalef(3.0, 0.8, 3.0);
   glutSolidCube(1.0);
   glPopMatrix();

   //ombro
   glTranslatef (0.0, 1.0, 0.0);
   glRotatef ((GLfloat) 90, 0.0, 0.0, 1.0);
   glRotatef ((GLfloat) shoulder, 0.0, 0.0, 1.0);
   glTranslatef (1.0, 0.0, 0.0);
   glPushMatrix();
   glScalef (4.0, 1.0, 1.5);
   glutSolidCube (1.0);
   glPopMatrix();

   //cotovelo
   glTranslatef (1.35, 0.5, 0.0);
   glRotatef ((GLfloat) 90, 0.0, 0.0, 1.0);
   glRotatef ((GLfloat) elbow, 0.0, 0.0, 1.0);
   glTranslatef (1.0, -1.0, 0.0);
   glPushMatrix();
   glScalef (4.0, 1.0, 1.5);
   glutSolidCube (1.0);
   glPopMatrix();

   //mao
   glTranslatef (2.0, 0.4, 0.0);
   glRotatef ((GLfloat) mao, 0.0, 0.0, 1.0);
   glPushMatrix();
   glScalef(1.6, 0.8, 1.7);
   glutSolidCube(1.0);
   glPopMatrix();

   //indicador
   glTranslatef (0.0, 0.4, 0.0);
   glRotatef ((GLfloat) 90, 0.0, 0.0, 1.0);
   glPushMatrix();
   glRotatef ((GLfloat) indicator, 0.0, 0.0, 1.0);
   glTranslatef (0.5, 0.2, 0.5);
   glScalef (2.0, 0.4, 0.4);
   glutSolidCube (0.5);
   glPopMatrix();

   //medio
   glPushMatrix();
   glRotatef ((GLfloat) middle, 0.0, 0.0, 1.0);
   glTranslatef (0.5, 0.2, -0.5);
   glScalef (2.0, 0.4, 0.4);
   glutSolidCube (0.5);
   glPopMatrix();

   //polegar
   glTranslatef (0.0, -0.3, 0.0);
   glPushMatrix();
   glRotatef ((GLfloat) polegar, 0.0, 0.0, 1.0);
   glTranslatef (0.5, -0.2, 0.0);
   glScalef (2.0, 0.4, 0.4);
   glutSolidCube (0.5);
   glPopMatrix();

  glPopMatrix();
  glutSwapBuffers();
}

void motion(int x, int y){

	if(mouseY > y && obs[1] < 25)
		obs[1] = obs[1] + 0.3;
	else if(mouseY < y && obs[1] > 1)
			obs[1] = obs[1] - 0.3;
	mouseY = y;
		
	if(mouseX > x)
		tetaxz=tetaxz+0.8;
	else if(mouseX < x)
			tetaxz=tetaxz-0.8;
	mouseX = x;
	glutPostRedisplay();
}

void GerenciaMouse(int button, int state, int x, int y)
{	
	if (button == GLUT_LEFT_BUTTON)
		if (state == GLUT_DOWN) {  // Zoom-in
			raioxz=raioxz-1;
    		glutPostRedisplay();
		}
	if (button == GLUT_RIGHT_BUTTON)
		if (state == GLUT_DOWN) {  // Zoom-out
			raioxz=raioxz+1;
      		glutPostRedisplay();
		}
	
	glutPostRedisplay();
}

void special(int key, int x, int y){
  switch (key) {
  case GLUT_KEY_UP: //------Movimenta o braco para cima-----
    if ((shoulder % 360)>-20) {
           shoulder = (shoulder - 5) % 360;
           mao = (mao + 5) % 360;
           if(pegar1 == 1){
           		coordEsf[1] += 0.35;
           		rxz1=rxz1-0.15;
           }else if(pegar2 == 1){
           			coordBule[1] += 0.75;
           			rxz=rxz-0.3;
           		}else if(pegar3 == 1){
           				coordCubo[1] += 0.35;
           				rxz3=rxz3-0.15;
           			}
           glutPostRedisplay();	
	}
    break;
  case GLUT_KEY_DOWN: //------Movimenta o braco para baixo-----
    if ((shoulder % 360)<45) {
           shoulder = (shoulder + 5) % 360;
           mao = (mao - 5) % 360;
           if(pegar1 == 1){
           		coordEsf[1] -= 0.35;
           		rxz1 = rxz1 + 0.15;
           }else if(pegar2 == 1){
           			coordBule[1] -= 0.75;
           			rxz = rxz + 0.3;
           		}else if(pegar3 == 1){
           				coordCubo[1] -= 0.35;
           				rxz3 = rxz3 + 0.15;
           			}
           glutPostRedisplay();
	}
    break;
  case GLUT_KEY_LEFT: //------Movimenta o braco para esquerda-----
    baseY = (baseY - 5) % 360;
    if(pegar1 == 1)
    	txz1=txz1+5;
    else if(pegar2 == 1)
    		txz=txz+5;
    	 else if(pegar3 == 1)
    			txz3=txz3+5;
    glutPostRedisplay();
    break;
  case GLUT_KEY_RIGHT: //------Movimenta o braco para direita-----
    baseY = (baseY + 5) % 360;
    if(pegar1 == 1)
    	txz1=txz1-5;
    else if(pegar2 == 1)
    		txz=txz-5;
    	 else if(pegar3 == 1)
    			txz3=txz3-5;
    glutPostRedisplay();
    break;
  }
}

void keyboard(unsigned char key, int x, int y){
  switch (key) {
  
     case 'e': //movimenta o cotovelo
	 	if ((elbow % 360)<35) {
           elbow = (elbow + 5) % 360;
           mao = (mao - 5) % 360;
           if(pegar1 == 1){
           		coordEsf[1] -= 0.2;
           		rxz1=rxz1+0.12;
           }else if(pegar2 == 1){
           			coordBule[1] -= 0.5;
           			rxz=rxz+0.2;
           		}else if(pegar3 == 1){
           				coordCubo[1] -= 0.2;
           				rxz3=rxz3+0.12;
           			}
           glutPostRedisplay();
 	 	}
        break;
     case 'E': //movimenta o cotovelo
	 	if ((elbow % 360)>-20) {
           elbow = (elbow - 5) % 360;
           mao = (mao + 5) % 360;
           if(pegar1 == 1){
           		coordEsf[1] += 0.2;
           		rxz1=rxz1-0.12;
           }else if(pegar2 == 1){
           			coordBule[1] += 0.5;
           			rxz=rxz-0.2;
           		}else if(pegar3 == 1){
           				coordCubo[1] += 0.2;
           				rxz3=rxz3-0.12;
           			}
           glutPostRedisplay();
	 	}
        break;
     case 'a': //abre os dedos
	 	if ((indicator % 360)<30) {
           indicator = (indicator + 5) % 360;
           middle = (middle + 5) % 360;
           polegar = (polegar - 5) % 360;
           glutPostRedisplay();
 	 	}
        break;
     case 's': //fecha os dedos
	 	if ((indicator % 360)>-10) {
           indicator = (indicator - 5) % 360;
           middle = (middle - 5) % 360;
           polegar = (polegar + 5) % 360;
           glutPostRedisplay();
 	 	}
 	 	break;
 	 
 	 case '1': //pega o objeto 1
 	 	pegar1 = 1;
 	 	pegar2 = pegar3 = 0;
 	 	break;
 	 case '2': //pega o objeto 2
 	 	pegar2 = 1;
 	 	pegar1 = pegar3 = 0;
 	 	break;
 	 case '3': //pega o objeto 3
 	 	pegar3 = 1;
 	 	pegar1 = pegar2 = 0;
 	 	break;
 	 case '0': //solta todos os objetos
 	 	pegar1 = pegar2 = pegar3 = 0;
 	 	break;
 	 	
     case 27:
        exit(0);
        break;
     default:
        break;
  }
}


void init(){
  glEnable(GL_DEPTH_TEST);
  glEnable(GL_BLEND);
  glBlendFunc(GL_SRC_ALPHA,GL_ONE_MINUS_SRC_ALPHA);

  glLightfv(GL_LIGHT0, GL_DIFFUSE, cor_luz0);
  glLightfv(GL_LIGHT0, GL_SPECULAR, cor_luz0);
  glLightfv(GL_LIGHT0, GL_AMBIENT, cor_luz0_amb);
  glLightfv(GL_LIGHT0, GL_POSITION, posicao_luz0);

  glEnable(GL_LIGHTING);
  glEnable(GL_LIGHT0);
  glEnable(GL_LIGHT1);

  glEnable(GL_AUTO_NORMAL);
  glEnable(GL_NORMALIZE);
  carregar_texturas();

}


int main(int argc,char **argv){
  glutInitWindowPosition(0,0);
  glutInitWindowSize(WIDTH,HEIGHT);
  glutInit(&argc,argv);
  glutInitDisplayMode(GLUT_RGB|GLUT_DEPTH|GLUT_DOUBLE);

  if(!glutCreateWindow("Robot Arm with Objects")) {
    fprintf(stderr,"Error opening a window.\n");
    exit(-1);
  }

  init();
  
  glutMotionFunc(motion);
  glutMouseFunc(GerenciaMouse);
  glutKeyboardFunc(keyboard);
  glutSpecialFunc(special);
  glutDisplayFunc(display);
  glutReshapeFunc(reshape);

  glutMainLoop();
  return(0);
}
