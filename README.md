#include <GL/glut.h>
#include<math.h>
#include<stdio.h>
#define num_of_ball 8

struct kad{
	int status;
	float x;
	float y;
	float z;
	float r;
	float g;
	float b;
	int flagLeft;
	int flagRight;
	int flagOut;
	int flagIn;

};
struct kad arr[num_of_ball];

//left למעלה
//in למטה
//ימינה o
//שמאלה i
int rot = 0;
float  zirx = 1, ziry = 0, zirz = 0;
int distance_arrball_to_white_ball = 10;
float revahX = 1.2, revahZ = 0.6;
float  v = 0.03;
char str[100], *str2;
int grade = 0;

void hole()
{

	double a, x, y;
	glRotatef(90, 1, 0, 0);
	glBegin(GL_POLYGON); /// glBegin(GL_LINE_LOOP);
	for (int i = 0; i <= 720; i++)
	{

		a = 3.14 * i;
		x = cos(a / 360);
		y = sin(a / 360);
		glVertex3d(x, y, 0);
	}

	glEnd();
	glRotatef(-90, 1, 0, 0);

}
void init_kad()
{
	////////////row1/////////////
	arr[0].status = 1;
	arr[0].x = -10.0;
	arr[0].y = 0.0;
	arr[0].z = 0;
	arr[0].r = 1.0;
	arr[0].g = 1.0;
	arr[0].b = 1.0;
	arr[0].flagRight = 0;
	arr[0].flagLeft = 0;
	arr[0].flagIn = 0;
	arr[0].flagOut = 0;
	///////////////////////
	arr[1].status = 1;
	arr[1].x = distance_arrball_to_white_ball;
	arr[1].y = 0.0;
	arr[1].z = 0;
	arr[1].r = 0.5;
	arr[1].g = 1;
	arr[1].b = 1;
	arr[1].flagRight = 0;
	arr[1].flagLeft = 0;
	arr[1].flagIn = 0;
	arr[1].flagOut = 0;
	////////////////////////////
	arr[2].x = distance_arrball_to_white_ball + revahX;
	arr[2].y = 0.0;
	arr[2].z = revahZ;
	arr[2].status = 1;
	arr[2].r = 1;
	arr[2].g = 0;
	arr[2].b = 0;
	arr[2].flagRight = 0;
	arr[2].flagLeft = 0;
	arr[2].flagIn = 0;
	arr[2].flagOut = 0;
	/////////////
	arr[3].x = distance_arrball_to_white_ball + revahX;
	arr[3].y = 0.0;
	arr[3].z = -revahZ;
	arr[3].status = 1;
	arr[3].r = 1;
	arr[3].g = 1;
	arr[3].b = 0;
	arr[3].flagRight = 0;
	arr[3].flagLeft = 0;
	arr[3].flagIn = 0;
	arr[3].flagOut = 0;
	/////////////
	arr[4].x = distance_arrball_to_white_ball + revahX + revahX;
	arr[4].y = 0.0;
	arr[4].z = -revahZ - revahZ;
	arr[4].status = 1;
	arr[4].r = 0;
	arr[4].g = 1;
	arr[4].b = 0;
	arr[4].flagRight = 0;
	arr[4].flagLeft = 0;
	arr[4].flagIn = 0;
	arr[4].flagOut = 0;
	/////////////
	arr[5].x = distance_arrball_to_white_ball + revahX + revahX;
	arr[5].y = 0.0;
	arr[5].z = 0;
	arr[5].status = 1;
	arr[5].r = 0;
	arr[5].g = 0;
	arr[5].b = 1;
	arr[5].flagRight = 0;
	arr[5].flagLeft = 0;
	arr[5].flagIn = 0;
	arr[5].flagOut = 0;
	////////////row2/////////////
	arr[6].x = distance_arrball_to_white_ball + revahX + revahX;
	arr[6].y = 0.0;
	arr[6].z = revahZ + revahZ;
	arr[6].status = 1;
	arr[6].r = 1.0;
	arr[6].g = 0.5;
	arr[6].b = 0.0;
	arr[6].flagRight = 0;
	arr[6].flagLeft = 0;
	arr[6].flagIn = 0;
	arr[6].flagOut = 0;
	/////////////
	arr[7].status = 1;
	arr[7].x = distance_arrball_to_white_ball - +revahX;
	arr[7].y = 0.0;
	arr[7].z = 0;
	arr[7].r = 0;
	arr[7].g = 0;
	arr[7].b = 0;
	arr[7].flagRight = 0;
	arr[7].flagLeft = 0;
	arr[7].flagIn = 0;
	arr[7].flagOut = 0;

}
void keyboard(unsigned char key, int x, int y)
{
	// esc to quit
	if (key == 27) exit(1);
	// rotate
	if (key == 'w'){
		zirx = 1, ziry = 0, zirz = 0;
		rot += 5;
		rot %= 360;
	}
	if (key == 'q'){

		zirx = 0, ziry = 1, zirz = 0;

		rot += 5;
		rot %= 360;
	}
	// change rotate axis
	if (key == 'e')
	{
		zirx = 0, ziry = 0, zirz = 1;

		rot += 5;
		rot %= 360;


	}
	if (key == 'l') { arr[0].flagLeft = 1, arr[0].flagRight = 0; }  /*למעלה*/
	if (key == 'r') { arr[0].flagRight = 1, arr[0].flagLeft = 0; }  /*למטה*/
	if (key == 'i') { arr[0].flagIn = 1, arr[0].flagOut = 0; }      /*שמאלה*/
	if (key == 'o') { arr[0].flagOut = 1, arr[0].flagIn = 0; }        /*ימינה לכייון הכדורים*/

	glutPostRedisplay();


}
void arrBall()
{
	glTranslatef(0, 7, 0);
	glTranslatef(0, 1, 0);
	for (int i = 0; i<num_of_ball; i++)
	{
		if (arr[i].status == 1)
		{
			glColor3f(arr[i].r, arr[i].g, arr[i].b);
			glTranslatef(arr[i].x, arr[i].y, arr[i].z);
			glutSolidSphere(0.5, 32, 32);
			glTranslatef(-arr[i].x, -arr[i].y, -arr[i].z);
		}
	}

}
void ritzpa()
{
	/************************************************************/
	// Floor
	glColor3f(0.6, 0.26, 0.8);//sagol
	glTranslatef(0, -7, 0);
	glBegin(GL_QUADS);
	glNormal3f(1, 0, 0);	// normal straight up
	glVertex3f(-15, 0, -5);
	glVertex3f(15, 0, -5);
	glVertex3f(15, 0, 5);
	glVertex3f(-15, 0, 5);
	glEnd();
}
void LEG()
{
	glBegin(GL_QUADS);
	// Front

	glNormal3f(0, 0, 1);
	glVertex3f(-1, 0, 1);
	glVertex3f(-1, 7, 1);
	glVertex3f(1, 7, 1);
	glVertex3f(1, 0, 1);

	// Back
	glNormal3f(0, 0, -1);
	glVertex3f(-1, 0, -1);
	glVertex3f(1, 0, -1);
	glVertex3f(1, 7, -1);
	glVertex3f(-1, 7, -1);

	// Left side
	glNormal3f(-1, 0, 0);
	glVertex3f(-1, 0, 1);
	glVertex3f(-1, 7, 1);
	glVertex3f(-1, 7, -1);
	glVertex3f(-1, 0, -1);

	// Right side
	glNormal3f(1, 0, 0);
	glVertex3f(1, 0, 1);
	glVertex3f(1, 0, -1);
	glVertex3f(1, 7, -1);
	glVertex3f(1, 7, 1);
	glEnd();
}
void dafnaA()
{
	glBegin(GL_QUADS);
	// Front

	glNormal3f(0, 0, 1);
	glVertex3f(-15, 0, 0.5);
	glVertex3f(-15, 1, 0.5);
	glVertex3f(15, 1, 0.5);
	glVertex3f(15, 0, 0.5);

	// Back
	glNormal3f(0, 0, -1);
	glVertex3f(-15, 0, -0.5);
	glVertex3f(15, 0, -0.5);
	glVertex3f(15, 1, -0.5);
	glVertex3f(-15, 1, -0.5);

	// Left side
	glNormal3f(-1, 0, 0);
	glVertex3f(-15, 0, 0.5);
	glVertex3f(-15, 1, 0.5);
	glVertex3f(-15, 1, -0.5);
	glVertex3f(-15, 0, -0.5);

	// Right side
	glNormal3f(1, 0, 0);
	glVertex3f(15, 0, 0.5);
	glVertex3f(15, 0, -0.5);
	glVertex3f(15, 1, -0.5);
	glVertex3f(15, 1, 0.5);

	// up floor
	glNormal3f(0, 1, 0);
	glVertex3f(-15, 1, 0.5);
	glVertex3f(-15, 1, -0.5);
	glVertex3f(15, 1, -0.5);
	glVertex3f(15, 1, 0.5);


	glEnd();
}
void dafnaB()
{
	glBegin(GL_QUADS);
	// Front

	glNormal3f(0, 0, 1);
	glVertex3f(-1, 0, 10);
	glVertex3f(-1, 1, 10);
	glVertex3f(0, 1, 10);
	glVertex3f(0, 0, 10);

	// Back
	glNormal3f(0, 0, 1);
	glVertex3f(-1, 0, 1);
	glVertex3f(0, 0, 1);
	glVertex3f(0, 1, 1);
	glVertex3f(-1, 1, 1);

	// Left side
	glNormal3f(-1, 0, 0);
	glVertex3f(-1, 0, 10);
	glVertex3f(-1, 1, 10);
	glVertex3f(-1, 1, 0);
	glVertex3f(-1, 0, 0);

	// Right side
	glNormal3f(1, 0, 0);
	glVertex3f(0, 0, 10);
	glVertex3f(0, 0, 0);
	glVertex3f(0, 1, 0);
	glVertex3f(0, 1, 10);

	// up floor
	glNormal3f(0, 1, 0);
	glVertex3f(-1, 1, 10);
	glVertex3f(-1, 1, 0);
	glVertex3f(0, 1, 0);
	glVertex3f(0, 1, 10);

	glEnd();
}

void draw()
{
	glClear(GL_COLOR_BUFFER_BIT |
		GL_DEPTH_BUFFER_BIT);

	glLoadIdentity();
	glTranslatef(0, -3, -35);
	/*ניקוד*/
	sprintf(str, "score : %d", grade);
	str2 = str;
	glColor3f(1.0, 1.0, 0.0);
	glRasterPos3f(-4, 15, 0);
	do glutBitmapCharacter(GLUT_BITMAP_HELVETICA_18, *str2);
	while (*(++str2));


	glRotatef(rot, zirx, ziry, zirz);   /*zirx = 1, ziry = 0, zirz = 0;  'W' איזה ציר יסובב את השולחן*/

	/************************************************************/

	// Floor
	glBegin(GL_QUADS);
	glColor3f(0.2, 0.7, 0.3);
	glNormal3f(1, 0, 0);	// normal straight up
	glVertex3f(-15, 0, -5);
	glVertex3f(15, 0, -5);
	glVertex3f(15, 0, 5);
	glVertex3f(-15, 0, 5);
	glEnd();


	glColor3f(0.5f, 0.35f, 0.05f);
	glTranslatef(-14, -7, 4);
	LEG();
	glTranslatef(14, 7, -4);
	glTranslatef(14, -7, 4);
	LEG();
	glTranslatef(-14, 7, -4);
	glTranslatef(14, -7, -4);
	LEG();
	glTranslatef(-14, 7, 4);
	glTranslatef(-14, -7, -4);
	LEG();
	glTranslatef(14, 7, 4);
	glTranslatef(0, 0, 4.5);
	dafnaA();
	glTranslatef(0, 0, -4.5);
	glTranslatef(0, 0, -4.5);
	dafnaA();
	glTranslatef(0, 0, 4.5);
	glTranslatef(-14, 0, -5);
	dafnaB();
	glTranslatef(14, 0, 5);
	glTranslatef(15, 0, -5);
	dafnaB();
	glTranslatef(-15, 0, 5);
	ritzpa();
	arrBall();
	glTranslatef(-13, -0.9, 3);  /* חור שמאלי תחתון*/
	glColor3f(0, 0, 0);
	hole();
	glTranslatef(13, 0, -3);
	glTranslatef(13, 0, 3);         /*חור ימני תחתון*/
	glColor3f(1, 0, 0);
	hole();
	glTranslatef(-13, 0, -3);
	glTranslatef(-13, 0, -3);           /* חור שמאלי עליון*/
	glColor3f(1, 0, 0);
	hole();
	glTranslatef(13, 0, 3);
	glTranslatef(13, 0, -3);              /*חור ימני עליון*/
	glColor3f(0, 0, 0);
	hole();
	glTranslatef(-13, 0, 3);

	glutSwapBuffers();			// display the output


}



int move_hole(int i, int x, int z)    /*בדיקת התנגשות בין כדור לחור*/
{

	if (arr[i].x >= x - 0.9 && arr[i].x <= x + 0.9 && arr[i].z >= z - 0.9 && arr[i].z <= z + 0.9)
		return 0;

	return 1;
}

int move_x(int i, int j)  /* בדיקת התנגשות בין כדור לכדור*/
{
	if (j != i){
		if (arr[i].x >= arr[j].x - 0.9 && arr[i].x <= arr[j].x + 0.9 && arr[i].z >= arr[j].z - 0.9 && arr[i].z <= arr[j].z + 0.9)
			return 0;
	}
	return 1;
}

void Idle()
{
	int j;
	for (int i = 0; i < num_of_ball; i++)
	{
		if (arr[i].status == 1)   /* סטטוס1- הכדור מצויר*/
		{
			/*בדיקת התנגשות עם כדורים*/
			for (j = 0; j < num_of_ball; j++)
			{
				if (i != j && arr[j].status == 1 && move_x(i, j) == 0) /*קרתה התנגשות*/
				{          /* תנאי כל ההתנגשויות האפשריות*/

					if (arr[i].flagLeft)     /* כדור פוגע מלמטה*/
					{
						arr[i].z += v;   /* במקרה שנכנסו אחד בשני זו הפרדה*/
						arr[j].z -= v;
						arr[i].flagRight = 1;  arr[i].flagLeft = 0;
						arr[j].flagRight = 0;  arr[j].flagLeft = 1;
					}

					if (arr[i].flagRight)  /* כדור פוגע מלמעלה*/
					{
						arr[i].z -= v;
						arr[j].z += v;
						arr[i].flagRight = 0;  arr[i].flagLeft = 1;
						arr[j].flagRight = 1;  arr[j].flagLeft = 0;
					}


					if (arr[i].flagIn) /* כדור פוגע משמאל*/
					{
						arr[i].x += v;
						arr[j].x -= v;
						arr[i].flagIn = 0;  arr[i].flagOut = 1;
						arr[j].flagIn = 1;  arr[j].flagOut = 0;
					}

					if (arr[i].flagOut) /*כדור פוגע מימין*/
					{
						arr[i].x -= v;
						arr[j].x += v;
						arr[i].flagOut = 0;  arr[i].flagIn = 1;
						arr[j].flagOut = 1;  arr[j].flagIn = 0;
					}

				}
			}

			if (arr[i].flagOut)
			{
				if (arr[i].x <= 13.5 - v)
				{
					arr[i].x += v;
				}

				if (arr[i].x >= 13.5 - v)
				{
					arr[i].flagIn = 1; arr[i].flagOut = 0;
				}

			}



			if (arr[i].flagIn)
			{
				if (arr[i].x >= -13.5)
				{
					arr[i].x -= v;
				}
				if (arr[i].x <= -13.5)
				{
					arr[i].flagOut = 1; arr[i].flagIn = 0;
				}
			}

			if (arr[i].flagRight)
			{
				if (arr[i].z <= 3.5 - v)
				{
					arr[i].z += v;
				}
				if (arr[i].z >= 3.5 - v)
				{
					arr[i].flagRight = 0;  arr[i].flagLeft = 1;
				}
			}




			if (arr[i].flagLeft)
			{
				if (arr[i].z >= -3.5 + v)
				{
					arr[i].z -= v;
				}

				if (arr[i].z <= -3.5 + v)
				{
					arr[i].flagRight = 1;  arr[i].flagLeft = 0;
				}

			}

			/*	התנגשות בין חורים*/

			if (move_hole(i, -13, 3) == 0) /*חור שמאלי תחתון*/
			{
				if (i == 0)
				{
					arr[i].x = -10;
					arr[i].z = 0;
					arr[i].flagIn = 0;
					arr[i].flagOut = 0;
					arr[i].flagLeft = 0;
					arr[i].flagRight = 0;
					grade = grade - 5;

				}
				else
				{
					arr[i].status = 0;
					grade = grade - 10;
				}
			}



			if (move_hole(i, 13, 3) == 0) /*חור ימני תחתון*/
			{
				if (i == 0)
				{
					arr[i].x = -10;
					arr[i].z = 0;
					arr[i].flagIn = 0;
					arr[i].flagOut = 0;
					arr[i].flagLeft = 0;
					arr[i].flagRight = 0;
					grade = grade - 5;
				}
				else
				{
					arr[i].status = 0;
					grade = grade + 10;
				}
			}

			if (move_hole(i, 13, -3) == 0) /*חור ימני עליון*/
			{
				if (i == 0)
				{
					arr[i].x = -10;
					arr[i].z = 0;
					arr[i].flagIn = 0;
					arr[i].flagOut = 0;
					arr[i].flagLeft = 0;
					arr[i].flagRight = 0;
					grade = grade - 5;
				}
				else
				{
					arr[i].status = 0;
					grade = grade - 10;
				}
			}

			if (move_hole(i, -13, -3) == 0) /*חור שמאלי עליון*/
			{
				if (i == 0)
				{
					arr[i].x = -10;
					arr[i].z = 0;
					arr[i].flagIn = 0;
					arr[i].flagOut = 0;
					arr[i].flagLeft = 0;
					arr[i].flagRight = 0;
					grade = grade - 5;
				}
				else
				{
					arr[i].status = 0;
					grade = grade + 10;
				}
			}








			draw();
		}
	}
}




// Set OpenGL parameters
void init()
{
	glMatrixMode(GL_PROJECTION);
	glLoadIdentity();
	gluPerspective(60, 1.0, 0.1, 100);
	glMatrixMode(GL_MODELVIEW);

	// Lighting parameters

	GLfloat mat_ambdif[] = { 1.0, 1.0, 1.0, 1.0 };
	GLfloat mat_specular[] = { 0.0, 1.0, 0.0, 0.0 };
	GLfloat mat_shininess[] = { 80.0 };
	GLfloat light_position[] = { 1.0, 1.0, 1.0, 0.0 };
	glClearColor(0.0, 0.0, 0.0, 0.0);


	glMaterialfv(GL_FRONT, GL_AMBIENT_AND_DIFFUSE, mat_ambdif);	// set both amb and diff components
	glMaterialfv(GL_FRONT, GL_SPECULAR, mat_specular);		// set specular
	glMaterialfv(GL_FRONT, GL_SHININESS, mat_shininess);		// set shininess
	glLightfv(GL_LIGHT0, GL_POSITION, light_position);		// set light "position", in this case direction
	glColorMaterial(GL_FRONT, GL_AMBIENT_AND_DIFFUSE);		// active material changes by glColor3f(..)

	glEnable(GL_LIGHTING);
	glEnable(GL_LIGHT0);
	glEnable(GL_COLOR_MATERIAL);

	glEnable(GL_DEPTH_TEST);
}

int main(int argc, char *argv[])

{
	glutInit(&argc, argv);
	glutInitDisplayMode(GLUT_RGB | GLUT_DOUBLE | GLUT_DEPTH);	// RGB display, double-buffered, with Z-Buffer
	glutInitWindowSize(500, 500);					// 500 x 500 pixels
	glutCreateWindow("created by netanyahu");
	glutDisplayFunc(draw);						// Set the display function
	glutKeyboardFunc(keyboard);					// Set the keyboard function
	init_kad();
	glutIdleFunc(Idle);
	init();
	glutMainLoop();							// Start the main event loop
}

















