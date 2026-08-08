#include <windows.h>
#include <iostream>
#include <GL/glut.h>
#include <math.h>

using namespace std;

#define PI 3.14159265358979323846
#define DEG2RAD 3.14159 / 180.0

GLfloat x, y, radius, twicePi; // for circle
int triangleAmount;

GLfloat position_c3 = -0.9f; // for car 3
GLfloat speed_c3 = 0.01f;

GLfloat position_c4 = 0.9f; // for car 4
GLfloat speed_c4 = 0.01f;

GLfloat position_fish = -0.9f; // for car 4
GLfloat speed_fish = 0.0007f;

int cnt = 0, flag = 0, r = 0;

char *c;
/* Handler for window-repaint event. Call back when the window first appears and
whenever the window needs to be re-painted. */


void drawQuad(float x, float y, float xLength, float yLength) {
    glBegin(GL_QUADS);
    glVertex2f(x, y);
    glVertex2f(x + xLength, y);
    glVertex2f(x + xLength, y - yLength);
    glVertex2f(x, y - yLength);
    glEnd();
}

void drawLine(float x1, float y1, float x2, float y2) {
    glBegin(GL_LINES);
    glVertex2f(x1, y1);
    glVertex2f(x2, y2);
    glEnd();
}

void drawTriangle(float x1, float y1, float x2, float y2, float x3, float y3) {
    glBegin(GL_TRIANGLES);
    glVertex2f(x1, y1);
    glVertex2f(x2, y2);
    glVertex2f(x3, y3);
    glEnd();
}

void DrawEllipse(float posx, float posy, float radiusX, float radiusY) {
    const int numSegments = 50;
    glBegin(GL_POLYGON);
    for (int i = 0; i < numSegments; i++) {
        float theta = 2.0f * PI * float(i) / float(numSegments);
        float x = radiusX * cosf(theta);
        float y = radiusY * sinf(theta);
        glVertex2f(x + posx, y + posy);
    }
    glEnd();
}

void drawFish(float x, float y) {
    glPushMatrix();
    glTranslatef(x, y + position_fish, 0.0f);
    glScalef(0.003,0.003,0);
    glRotatef(90, 0.0f, 0.0f, 1.0f);

    // Fish body
    glColor3f(1.0, 0.5, 0.0); // Orange color
    DrawEllipse(0,0, 20, 10);

    // Fish tail
    glColor3f(1.0, 0.3, 0.0); // Darker orange
    glBegin(GL_TRIANGLES);
    glVertex2f(-20, 0);
    glVertex2f(-35, 10);
    glVertex2f(-35, -10);
    glEnd();

    // Fish eye
    glColor3f(0, 0, 0); // Black color
    DrawEllipse(10, 5, 2, 2);

    glPopMatrix();
}

void tree(int type, float r, float g, float b) {
    glLineWidth(10.0f);

    // Draw trunk
        glColor3ub(153, 51, 51);
        drawLine(0.0f, 0.25f,0.0f, 0.0f);

    glColor3ub(r, g, b);

    if (type == 1) {// Circle tree
        DrawEllipse(0.05f, 0.27f, 0.08f, 0.08f);
        DrawEllipse(0.0f, 0.3f, 0.08f, 0.08f);
        DrawEllipse(-0.05f, 0.27f, 0.08f, 0.08f);

        // Fruits
        glPointSize(5.0f);
        glBegin(GL_POINTS);
            glColor3f(1.0f, 0.0f, 0.0f);
            glVertex2f(-0.03f, 0.3f);
            glVertex2f(0.03f, 0.25f);
        glEnd();
    } else if (type == 2) {// Triangle tree
        drawTriangle(-0.1f, 0.15f, 0.0f, 0.4f, 0.1f, 0.15f);
    }
}

void shop() {
    // Left shop
    glColor3ub(255, 255, 255);
    drawQuad(-1.95f, 0.75f, 0.4f, 0.25f);

    glColor3ub(128, 0, 0);
    drawQuad(-1.95f, 0.85f, 0.4f, 0.1f);

    glBegin(GL_POLYGON); // shelter
    glColor3ub(255, 0, 0);
    glVertex2f(-2.0f, 0.65f);
    glVertex2f(-1.5f, 0.65f);
    glVertex2f(-1.5f, 0.7f);
    glVertex2f(-1.55f, 0.75f);
    glVertex2f(-1.95f, 0.75f);
    glVertex2f(-2.0f, 0.7f);
    glEnd();

    glBegin(GL_POLYGON); // door
    glColor3ub(0, 230, 230);
    glVertex2f(-1.9f, 0.5f);
    glVertex2f(-1.9f, 0.62f);
    glColor3ub(0, 153, 153);
    glVertex2f(-1.82f, 0.62f);
    glVertex2f(-1.82f, 0.5f);
    glEnd();

    glBegin(GL_POLYGON); // window
    glColor3ub(0, 230, 230);
    glVertex2f(-1.77f, 0.55f);
    glVertex2f(-1.77f, 0.62f);
    glColor3ub(0, 153, 153);
    glVertex2f(-1.6f, 0.62f);
    glVertex2f(-1.6f, 0.55f);
    glEnd();

    glColor3ub(128, 0, 0);
    drawLine(-1.52f, 0.5f, -1.98f, 0.5f);
    drawLine(-1.524f, 0.504f, -1.98f, 0.504f);
    drawLine(-1.9f, 0.5f, -1.9f, 0.62f);
    drawLine(-1.9f, 0.62f, -1.82f, 0.62f);
    drawLine(-1.82f, 0.62f, -1.82f, 0.5f);
    drawLine(-1.82f, 0.5f, -1.9f, 0.5f);
    drawLine(-1.82f, 0.55f, -1.9f, 0.55f);
    drawLine(-1.77f, 0.55f, -1.77f, 0.62f);
    drawLine(-1.77f, 0.62f, -1.6f, 0.62f);
    drawLine(-1.6f, 0.62f, -1.6f, 0.55f);
    drawLine(-1.6f, 0.55f, -1.77f, 0.55f);

    glColor3f(1.0, 1.0, 1.0);
    const char *c = "RFC";
    glRasterPos2f(-1.8, 0.78);
    for (int i = 0; c[i] != '\0'; i++)
        glutBitmapCharacter(GLUT_BITMAP_HELVETICA_18, c[i]);

    // Right shop
    glColor3ub(255, 204, 102);
    drawQuad(-0.75f, 0.8f, 0.55f, 0.3f);

    // Roof
    glColor3ub(179, 89, 0);
    drawQuad(-0.75f, 0.8f, 0.55f, -0.05f);

    glBegin(GL_POLYGON);
    glColor3ub(179, 89, 0);
    glVertex2f(-0.3f, 0.85f);
    glVertex2f(-0.35f, 0.9f);
    glVertex2f(-0.6f, 0.9f);
    glVertex2f(-0.66f, 0.85f);
    glEnd();

    // Inside
    glColor3ub(77, 38, 0);
    drawQuad(-0.73f, 0.75f, 0.51f, 0.15f);

    // Products
    glColor3ub(255, 0, 0);
    drawQuad(-0.7f, 0.65f, 0.1f, 0.05f);

    glColor3ub(255, 204, 102);
    drawQuad(-0.65f, 0.68f, 0.05f, 0.03f);

    drawQuad(-0.55f, 0.65f, 0.1f, 0.05f);

    glColor3ub(204, 0, 102);
    drawQuad(-0.4f, 0.65f, 0.05f, 0.05f); // Pink product

    drawQuad(-0.34f, 0.65f, 0.04f, 0.06f); // Product

    glColor3ub(128, 64, 0);
    drawQuad(-0.73f, 0.6f, 0.51f, 0.1f); // Table

    glBegin(GL_POLYGON); // Shelter
    glColor3ub(230, 115, 0);
    glVertex2f(-0.8f, 0.7f);
    glVertex2f(-0.15f, 0.7f);
    glVertex2f(-0.15f, 0.75f);
    glVertex2f(-0.2f, 0.8f);
    glVertex2f(-0.75f, 0.8f);
    glVertex2f(-0.8f, 0.75f);
    glEnd();

    glColor3ub(128, 0, 0);
    drawLine(-0.17f, 0.5f, -0.77f, 0.5f);
    drawLine(-0.17f, 0.504f, -0.77f, 0.504f);

    glColor3ub(255, 153, 51);
    drawLine(-0.73f, 0.6f, -0.22f, 0.6f);

    glColor3ub(255, 255, 0);
    const char *c2 = "Candy Shop";
    glRasterPos2f(-0.6, 0.84);
    for (int i = 0; c2[i] != '\0'; i++)
        glutBitmapCharacter(GLUT_BITMAP_9_BY_15, c2[i]);
}

void house(int r, int g, int b) {
    // Left house
    // Body
    glColor3ub(224, 228, 231);
    drawQuad(-1.4f, 1.0f, 0.5f, 0.5);

    // Windows
    for (float x = -1.35f; x <= -0.95f; x += 0.3f) {
        glBegin(GL_POLYGON); // Bottom window
        glColor3ub(r, g, b);
        glVertex2f(x, 0.55f);
        glVertex2f(x, 0.65f);
        glColor3ub(0, g, g);
        glVertex2f(x + 0.1f, 0.65f);
        glVertex2f(x + 0.1f, 0.55f);
        glEnd();

        glBegin(GL_POLYGON); // Top window
        glColor3ub(r, g, b);
        glVertex2f(x, 0.85f);
        glVertex2f(x, 0.95f);
        glColor3ub(0, g, g);
        glVertex2f(x + 0.1f, 0.95f);
        glVertex2f(x + 0.1f, 0.85f);
        glEnd();
    }

    // Door
    glColor3ub(77, 77, 77);
    drawQuad(-1.2f, 0.65f, 0.1f, 0.15f);

    glPointSize(5.0); // Door handle
    glBegin(GL_POINTS);
    glColor3f(1.0f, 1.0f, 1.0f);
    glVertex2f(-1.17f, 0.58f);
    glEnd();

    // Lines
    glColor3ub(77, 195, 255);
    for (float x = -1.35f; x <= -0.95f; x += 0.3f) {
        drawLine(x, 0.55f, x, 0.65f);
        drawLine(x, 0.65f, x + 0.1f, 0.65f);
        drawLine(x + 0.1f, 0.65f, x + 0.1f, 0.55f);
        drawLine(x + 0.1f, 0.55f, x, 0.55f);
        drawLine(x, 0.6f, x + 0.1f, 0.6f);
    }

    // Roof
    glColor3ub(128, 128, 128);
    drawLine(-1.4f, 0.5f, -1.4f, 1.0f);
    drawLine(-1.4f, 1.0f, -0.9f, 1.0f);
    drawLine(-0.9f, 1.0f, -0.9f, 0.5f);

    glColor3ub(128, 0, 0);
    drawLine(-0.87f, 0.5f, -1.43f, 0.5f);

    glBegin(GL_POLYGON); // Shelter
    glColor3ub(76, 174, 218);
    glVertex2f(-1.4f, 0.8f);
    glVertex2f(-1.45f, 0.75f);
    glVertex2f(-1.45f, 0.7f);
    glVertex2f(-0.85f, 0.7f);
    glVertex2f(-0.85f, 0.75f);
    glVertex2f(-0.9f, 0.8f);
    glEnd();

    // Right house
    // Body
    glColor3ub(153, 51, 0);
    drawQuad(-0.15f, 0.8f, 0.5f, 0.3f);

    // Store room
    glColor3ub(153, 51, 0);
    drawQuad(0.05f, 0.99f, 0.3f, 0.19f);

    // Windows
    for (float x = 0.05f; x <= 0.2f; x += 0.15f) {
        glBegin(GL_POLYGON);
        glColor3ub(r, g, b);
        glVertex2f(x, 0.6f);
        glVertex2f(x, 0.7f);
        glColor3ub(26, 140, 255);
        glVertex2f(x + 0.1f, 0.7f);
        glVertex2f(x + 0.1f, 0.6f);
        glEnd();
    }

    glBegin(GL_POLYGON); // Store room window
    glColor3ub(r, g, b);
    glVertex2f(0.15f, 0.85f);
    glVertex2f(0.15f, 0.95f);
    glColor3ub(26, 140, 255);
    glVertex2f(0.25f, 0.95f);
    glVertex2f(0.25f, 0.85f);
    glEnd();

    // Door
    glColor3ub(77, 77, 77);
    drawQuad(-0.1f, 0.65f, 0.1f, 0.15f);

    glPointSize(5.0); // Door handle
    glBegin(GL_POINTS);
    glColor3f(1.0f, 1.0f, 1.0f);
    glVertex2f(-0.03f, 0.6f);
    glEnd();

    glBegin(GL_POLYGON); // Door shelter
    glColor3ub(230, 115, 0);
    glVertex2f(0.02f, 0.65f);
    glVertex2f(-0.05f, 0.7f);
    glVertex2f(-0.12f, 0.65f);
    glEnd();

    // Lines
    glColor3ub(255, 153, 51);
    drawLine(-0.1f, 0.5f, -0.1f, 0.65f);
    drawLine(-0.1f, 0.65f, 0.0f, 0.65f);
    drawLine(0.0f, 0.65f, 0.0f, 0.5f);
    drawLine(0.0f, 0.5f, -0.1f, 0.5f);
    drawLine(0.02f, 0.65f, -0.05f, 0.7f);
    drawLine(-0.05f, 0.7f, -0.12f, 0.65f);
    drawLine(-0.12f, 0.65f, 0.02f, 0.65f);
    drawLine(0.36f, 0.8f, -0.16f, 0.8f);
    drawLine(0.36f, 0.84f, -0.16f, 0.84f);
    drawLine(-0.14f, 0.8f, -0.14f, 0.84f);
    drawLine(-0.1f, 0.8f, -0.1f, 0.84f);
    drawLine(-0.05f, 0.8f, -0.05f, 0.84f);
    drawLine(-0.0f, 0.8f, -0.0f, 0.84f);
    drawLine(0.05f, 0.8f, 0.05f, 0.84f);
    drawLine(0.1f, 0.8f, 0.1f, 0.84f);
    drawLine(0.15f, 0.8f, 0.15f, 0.84f);
    drawLine(0.2f, 0.8f, 0.2f, 0.84f);
    drawLine(0.25f, 0.8f, 0.25f, 0.84f);
    drawLine(0.3f, 0.8f, 0.3f, 0.84f);
    drawLine(0.35f, 0.8f, 0.35f, 0.84f);
    drawLine(-0.16f, 0.504f, 0.36f, 0.504f);
    drawLine(-0.16f, 0.5f, 0.36f, 0.5f);
    drawLine(0.04f, 0.99f, 0.36f, 0.99f);
    drawLine(0.04f, 0.985f, 0.36f, 0.985f);
}

void road_footpath() {
    // Road
    glColor3ub(95, 96, 91);
    drawQuad(0.4f, 1.0f, 1.0f, 2.0f);
    drawQuad(0.0f, 0.4f, 0.8f, 4.0f);

    // Footpath
    glColor3ub(176, 191, 189);
    drawQuad(-2.0f, 0.5f, 2.6f, 0.2f);
    drawQuad(0.4f, -0.45f, 0.2f, 0.6f);
    drawQuad(1.2f, 3.0f, 0.2f, 4.0f);
    drawQuad(0.4f, 1.0f, 0.2f, 0.9f);

    // Divider lines
    glLineWidth(3.0f);
    glColor3ub(255, 255, 255);
    drawLine(0.9f, 0.8f, 0.9f, 0.97f);
    drawLine(0.9f, -0.97f, 0.9f, -0.8f);

    // Zebra-crossing lines
    glLineWidth(5.0f);
    glColor3ub(255, 255, 255);
    drawLine(0.6f, 0.5f, 1.2f, 0.5f);
    drawLine(0.6f, 0.7f, 1.2f, 0.7f);
    drawLine(0.6f, -0.5f, 1.2f, -0.5f);
    drawLine(0.6f, -0.7f, 1.2f, -0.7f);

    glLineWidth(10.0f);
    for (float x = 0.7f; x <= 1.1f; x += 0.1f) {
        drawLine(x, -0.5f, x, -0.7f);
        drawLine(x, 0.5f, x, 0.7f);
    }
}

void traffic_light_rgb(float x, float y) {
    glColor3ub(204, 0, 0);
    DrawEllipse(x, y, 0.02f, 0.02f);
    glColor3ub(255, 204, 0);
    DrawEllipse(x, y - 0.05, 0.02f, 0.02f);
    glColor3ub(0, 128, 0);
    DrawEllipse(x, y - 0.1, 0.02f, 0.02f);
}

void traffic_light1() {

    // Stand
    glColor3ub(128, 0, 0);
    drawQuad(0.45f, 0.42f, 0.1f, 0.04f);

    // Pole
    glLineWidth(5.0f);
    glColor3ub(153, 0, 0);
    drawLine(0.55f, 0.4f, 0.7f, 0.4f);
    drawLine(0.7f, 0.4f, 0.7f, 0.15f);

    // Light
    glColor3ub(204, 122, 0);
    drawQuad(0.75f, 0.15f, -0.1f, 0.2f);

    // Light frame
    glLineWidth(3.0f);
    glColor3ub(0, 0, 0);
    drawLine(0.65f, 0.15f, 0.75f, 0.15f);
    drawLine(0.75f, 0.15f, 0.75f, -0.05f);
    drawLine(0.75f, -0.05f, 0.65f, -0.05f);
    drawLine(0.65f, -0.05f, 0.65f, 0.15f);
    drawLine(0.65f, 0.1f, 0.6f, 0.1f);
    drawLine(0.65f, 0.05f, 0.62f, 0.05f);
    drawLine(0.65f, 0.0f, 0.63f, 0.0f);
    drawLine(0.75f, 0.1f, 0.8f, 0.1f);
    drawLine(0.75f, 0.05f, 0.78f, 0.05f);
    drawLine(0.75f, 0.0f, 0.77f, 0.0f);

    // RGB lights
    traffic_light_rgb(0.7, 0.1);
}
void traffic_light2() {
    // Stand
    glColor3ub(128, 0, 0);
    drawQuad(1.28f, -0.35f, 0.04f, 0.1f);

    // Pole
    glLineWidth(5.0f);
    glColor3ub(153, 0, 0);
    drawLine(1.3f, -0.35f, 1.3f, -0.2f);
    drawLine(1.3f, -0.2f, 1.0f, -0.2f);

    // Light
    glColor3ub(204, 122, 0);
    drawQuad(0.9f, -0.1f, 0.1f, 0.2f);

    // Light frame
    glLineWidth(3.0f);
    glColor3ub(0, 0, 0);
    drawLine(0.9f, -0.1f, 1.0f, -0.1f);
    drawLine(1.0f, -0.1f, 1.0f, -0.3f);
    drawLine(1.0f, -0.3f, 0.9f, -0.3f);
    drawLine(0.9f, -0.3f, 0.9f, -0.1f);
    drawLine(0.9f, -0.15f, 0.85f, -0.15f);
    drawLine(0.9f, -0.2f, 0.87f, -0.2f);
    drawLine(0.9f, -0.25f, 0.88f, -0.25f);
    drawLine(1.0f, -0.15f, 1.05f, -0.15f);
    drawLine(1.0f, -0.2f, 1.03f, -0.2f);
    drawLine(1.0f, -0.25f, 1.02f, -0.25f);

    // RGB lights
    traffic_light_rgb(0.95, -0.15);
}

void road_light(int type) {
    glLineWidth(3.0f);

    // Draw left road border
    glColor3ub(128, 128, 128);
    drawLine(1.3f, -1.0f, 1.3f, -0.9f);
    drawLine(1.3f, -0.9f, 1.0f, -0.9f);

    // Draw left road divider
    if (type == 2){glColor3ub(255, 255, 0);}
    else{glColor3ub(191, 191, 191);}
    drawQuad(1.0f, -0.95f, 0.15f, -0.05f);

    // Draw top road divider
    glColor3ub(128, 128, 128);
    drawLine(0.5f, 0.8f, 0.5f, 0.9f);
    drawLine(0.5f, 0.9f, 0.8f, 0.9f);

    // Draw top road divider
    if (type == 2){glColor3ub(255, 255, 0);}
    else{glColor3ub(191, 191, 191);}
    drawQuad(0.8f, 0.85f, -0.15f, -0.05f);

    // Draw point on top road divider
    glBegin(GL_POINTS);
    glColor3ub(89, 89, 89);
    glVertex2f(0.5f, 0.8f);
    glEnd();

    // Draw right road divider
    glColor3ub(128, 128, 128);
    drawLine(1.3f, 0.5f, 1.3f, 0.6f);
    drawLine(1.3f, 0.6f, 1.0f, 0.6f);

    // Draw right road divider
    if (type == 2){glColor3ub(255, 255, 0);}
    else{glColor3ub(191, 191, 191);}
    drawQuad(1.0f, 0.55f, 0.15f, -0.05f);

    // Draw point on right road divider
    glBegin(GL_POINTS);
    glColor3ub(89, 89, 89);
    glVertex2f(1.3f, 0.5f);
    glEnd();
}

void drawCarBody() {
    glBegin(GL_POLYGON); // body
    glColor3ub(255, 255, 255);
    glVertex2f(-0.13f, 0.25f);
    glVertex2f(0.07f, 0.25f);
    glVertex2f(0.15f, 0.23f);
    glVertex2f(0.17f, 0.2f);
    glVertex2f(0.17f, 0.1f);
    glVertex2f(0.15f, 0.07f);
    glVertex2f(0.07f, 0.05f);
    glVertex2f(-0.13f, 0.05f);
    glVertex2f(-0.15f, 0.07f);
    glVertex2f(-0.15f, 0.23f);
    glEnd();
}

void drawCarWindow() {
    glColor3ub(0, 0, 0);
    drawQuad(-0.13f, 0.24f, 0.2f, 0.18f);
}

void drawCarRoof() {
    glColor3ub(255, 255, 255);
    drawQuad(-0.1f, 0.21f, 0.13f, 0.12f);
}

void drawCarLights() {
    glBegin(GL_POLYGON); // light
    glColor3ub(242, 242, 242);
    glVertex2f(0.15f, 0.23f);
    glVertex2f(0.17f, 0.2f);
    glVertex2f(0.14f, 0.2f);
    glVertex2f(0.14f, 0.23f);
    glEnd();
    glBegin(GL_POLYGON); // light
    glColor3ub(242, 242, 242);
    glVertex2f(0.15f, 0.07f);
    glVertex2f(0.17f, 0.1f);
    glVertex2f(0.14f, 0.1f);
    glVertex2f(0.14f, 0.07f);
    glEnd();
}

void drawCarHeadLights() {
    if (flag != 0) {
        glBegin(GL_POLYGON); // head-light
        glColor3ub(255, 255, 204);
        glVertex2f(0.17f, 0.2f);
        glVertex2f(0.14f, 0.23f);
        glVertex2f(0.45f, 0.3f);
        glVertex2f(0.45f, 0.1f);
        glEnd();
        glBegin(GL_POLYGON); // head-light
        glColor3ub(255, 255, 204);
        glVertex2f(0.17f, 0.1f);
        glVertex2f(0.14f, 0.07f);
        glVertex2f(0.45f, 0.0f);
        glVertex2f(0.45f, 0.2f);
        glEnd();
    }
}

void drawCarLines() {
    glColor3ub(255, 255, 255);
    drawLine(0.07f, 0.24f, 0.03f, 0.21f);
    drawLine(0.03f, 0.09f, 0.07f, 0.06f);
    drawLine(-0.13f, 0.06f, -0.1f, 0.09f);
    drawLine(-0.1f, 0.21f, -0.13f, 0.24f);
    drawLine(-0.03f, 0.24f, -0.03f, 0.21f);
    drawLine(-0.03f, 0.09f, -0.03f, 0.06f);
}

void car() {
    drawCarBody();
    drawCarWindow();
    drawCarRoof();
    drawCarLights();
    drawCarHeadLights();
    drawCarLines();

}

void river(int type) {
    int g = 24;
    if (type == 1){
        g = 204;
}
    glPushMatrix();  // Save the current matrix

    // Translate to the right and upwards
    glTranslatef(0.5f, 0.5f, 0.0f);

    // Rotate 90 degrees counter-clockwise to make it vertical
    glRotatef(90.0f, 0.0f, 0.0f, 1.0f);

        glColor3ub(51, g, 255);
        drawQuad(-2.0f, -0.55f, 3.5f, 1.0f);

    glPopMatrix();  // Restore the previous matrix
}

void start() {
    // Clear the color buffer (background)
    glClear(GL_COLOR_BUFFER_BIT);

    // Set background color to white
    glClearColor(1.0f, 1.0f, 1.0f, 1.0f);

    // Draw a colored polygon
    glBegin(GL_POLYGON);
        glColor3ub(255, 255, 0);
        glVertex2f(-2.0f, 2.0f);
        glColor3ub(147, 112, 219);
        glVertex2f(2.0f, 2.0f);
        glColor3ub(30, 144, 255);
        glVertex2f(2.0f, -2.0f);
        glColor3ub(70, 130, 180);
        glVertex2f(-2.0f, -2.0f);
    glEnd();

    // Display text: Traffic Signal In The City
    glColor3ub(255, 255, 255);
    const char* c1 = "Traffic Signal In The City";
    glRasterPos2f(-0.3, 0.7);
    for (int i = 0; c1[i] != '\0'; i++)
        glutBitmapCharacter(GLUT_BITMAP_TIMES_ROMAN_24, c1[i]);

    // Display text: Features
    glColor3ub(0, 0, 0);
    const char* c2 = "Feature: \n-> Press 'R' = Move the cars in the HORIZONTAL. \n-> Press 'S' = Turn on GREEN signal and stop the cars.";
    float x = 0.3f;
    glRasterPos2f(-1.5, 0.3);
    for (int i = 0; c2[i] != '\0'; i++) {
        glutBitmapCharacter(GLUT_BITMAP_TIMES_ROMAN_24, c2[i]);
        if (c2[i] == '\n')
            glRasterPos2f(-1.5, x = x - 0.1);
    }

    // Display text: More Features
    glColor3ub(0, 0, 0);
    const char* c3 = "-> Press 'D' = Switch to DAY view. \n-> Press 'N' = Switch to NIGHT view.";
    glRasterPos2f(-1.5, x = x - 0.1);
    for (int i = 0; c3[i] != '\0'; i++) {
        glutBitmapCharacter(GLUT_BITMAP_TIMES_ROMAN_24, c3[i]);
        if (c3[i] == '\n')
            glRasterPos2f(-1.5, x = x - 0.1);
    }

    // Display text: Press F to START the project
    glColor3ub(255, 0, 0);
    const char* c4 = "Press F to START the project";
    glRasterPos2f(-0.3, -0.7);
    for (int i = 0; c4[i] != '\0'; i++)
        glutBitmapCharacter(GLUT_BITMAP_TIMES_ROMAN_24, c4[i]);

    glFlush();
}

void init() {
    // Set the window background color to white
    glClearColor(1.0, 1.0, 1.0, 0.0);

    // Set up the projection matrix
    glMatrixMode(GL_PROJECTION);
    glLoadIdentity(); // Reset transformations
    glOrtho(-200, 200, -200, 300, -200, 300); // Define the viewing volume
}

// Initial position of the character
float characterX = 0.0f;
float characterY = 80.0f;

// Scaling factors
float scale = 0.09f;
float scale2 = 0.1f;

void humanBody() {
    glPushMatrix();
    glTranslatef(characterX, characterY + 20, 0.0f);
    glScalef(scale, scale, 1.0f); // Apply scaling

    // Head
    glColor3f(1, 0.87, 0.77);
    DrawEllipse(5, 100, 20, 25); // Head shape
    glColor3f(0.88f, 0.55f, 0.0f);
    DrawEllipse(5, 115, 20, 10); // Hair

    // Facial features
    glColor3f(0, 0, 0); // Black color for eyes
    DrawEllipse(-5, 10, 52, 3); // Left eye
    DrawEllipse(15, 10, 52, 3); // Right eye
    glColor3f(1, 0, 0);
    DrawEllipse(10, 90, 3, 4); // Mouth

    // Neck
    glColor3f(1, 0.87, 0.77);
    drawQuad(-2.5, 80, 15, 20);

    // Arms
    glPushMatrix();
    glRotatef(-15, 0, 0, 1);
    drawQuad(-25, 60, -15, 70);
    glPopMatrix();

    glPushMatrix();
    glRotatef(15, 0, 0, 1);
    drawQuad(35, 60, 15, 70);
    glPopMatrix();

    // Legs
    glColor3f(52.0f / 255.0f, 83.0f / 255.0f, 109.0f / 255.0f);
    drawQuad(-20, 10, 20, 100);
    drawQuad(30, 10, -20, 100);

    // Torso
    glColor3f(216.0f / 255.0f, 219.0f / 255.0f, 214.0f / 255.0f);
    drawQuad(-20, 70, 50, 70);

    glPopMatrix();
    glPushMatrix();
    glTranslatef(characterX, characterY + 90, 0.0f);
    glScalef(scale2, scale2, 1.0f); // Apply scaling

    // Draw ground
    glColor3ub(255, 215, 0); // Gold
    drawQuad(-200, 100, 400, 400);
    glPopMatrix();
}

void circle() {
    GLfloat r1 = 30;

    // Draw the first ellipse
    glColor3f(10, 0, 0);
    DrawEllipse(0, 90, r1, r1);

    // Draw the second ellipse
    glColor3f(1, 1, 0);
    DrawEllipse(0, 100, r1, r1);

    glLineWidth(4);
    glColor3f(0.0f, 0.0f, 0.0f);
    glBegin(GL_LINE_LOOP);
    glVertex2f(-100, 200);
    glVertex2f(100, 200);
    glVertex2f(100, -60);
    glVertex2f(-100, -60);
    glEnd();
}

void drawSeat(float x, float y) {
    glColor3f(0.9f,0.9f,0.9f);
    drawQuad(x-5, y+5, 10, 10);
}

void drawPerson(float x, float y) {
    // Generate random RGB values
    float r = static_cast<float>(rand()) / RAND_MAX; // Random value between 0 and 1 for red component
    float g = static_cast<float>(rand()) / RAND_MAX; // Random value between 0 and 1 for green component
    float b = static_cast<float>(rand()) / RAND_MAX; // Random value between 0 and 1 for blue component
    glColor3f(r, g, b); // Random color
    glPushMatrix();
    glTranslatef(x, y + 7, 0); // Slightly above the seat
    glutSolidSphere(3, 20, 20); // Head of the person
    glPopMatrix();
}

void theater() {
    // Draw back curtain
    glColor3f(0.51, 0.0, 0.0); // Dark grey color for the curtain
    drawQuad(-100, 200, 200, 260);

    // Draw stage
    glColor3f(0.7, 0.0, 0.0);
    drawQuad(-90, 90, 180, 150);

    // Draw stage curtains
    glColor3f(0.7, 0.0, 0.0);
    drawQuad(-90, 200, 10, 110);
    drawQuad(80, 200, 10, 110);

    // Draw rows of seats
    int rows = 3;
    int cols = 6;
    float startX = -50.0f;
    float startY = 30.0f;
    float gapX = 20.0f;
    float gapY = 20.0f;

    for (int row = 0; row < rows; ++row) {
        for (int col = 0; col < cols; ++col) {
            float x = startX + col * gapX;
            float y = startY - row * gapY;
            drawSeat(x, y);
                drawPerson(x, y);
            }
        }
}
void music(){
    glColor3f(1, 1, 1);
    DrawEllipse(-5, 160,2, 2);
    DrawEllipse(5, 160,2, 2);
    drawLine(6,160,6,175);

    drawLine(-3,160,-3,175);

    drawLine(6,175,-6,175);

    glColor3f(0.0, 0.0, 0.0); // Black color
    drawLine(-6,80,-5,90);

    drawLine(-19,80,-18,90);


     // Draw piano body
    drawQuad(-20, 90, 15, 5);

    // Draw piano keys
    for (int i = -4; i <= -2; i++) {
        if (i % 2 == 0)
            glColor3f(1.0, 1.0, 1.0); // White keys
        else
            glColor3f(0.0, 0.0, 0.0); // Black keys

        drawQuad(i * 5, 95, 5, 5);
    }
    glPopMatrix();
}
void renderBitmapString(float x, float y, void *font, const char *string) {


    const char *c;
    glRasterPos2f(x, y);  // Set the position for the text
    for (c = string; *c != '\0'; c++) {
        glutBitmapCharacter(font, *c);  // Render each character
    }
}

void lights(){
    glPushMatrix();
    glColor3f(1.0, 0.8, 0.0);
    glBegin(GL_QUADS);
    glVertex2f(-80, 200);
    glVertex2f(-60, 160);
    glVertex2f(-70, 140);
    glVertex2f(-80, 140);
    glEnd();

    glBegin(GL_QUADS);
    glVertex2f(80, 200);
    glVertex2f(60, 160);
    glVertex2f(70, 140);
    glVertex2f(80, 140);
    glEnd();

    glColor3f(1.0, 1.0, 1.0); // White color for lights
    glBegin(GL_TRIANGLES);
    glVertex2f(65, 150);
    glVertex2f(10, 125);
    glVertex2f(30, 100);
    glEnd();

    glBegin(GL_TRIANGLES);
    glVertex2f(-65, 150);
    glVertex2f(-10, 125);
    glVertex2f(-30, 100);
    glEnd();

    glPopMatrix();
}

void fullTheater(){
    glPushMatrix(); // Save the current transformation matrix

    theater();
    circle();
    humanBody();
    music();
    lights();
    // Set the text color (RGB)
    glColor3f(1.0, 1.0, 1.0);
    renderBitmapString(-75.0f, 185.0f, GLUT_BITMAP_HELVETICA_18, "Welcome to Azhar, Elinna, & Joseph's Theater");

    glPopMatrix(); // Restore the previous transformation matrix
}

void time(int type) {// day or night
	glClear(GL_COLOR_BUFFER_BIT);         // Clear the color buffer (background)

    c="Closed";
    if(type == 1){
    c="Open";
    // Background
    glColor3ub(51, 204, 51);
    }
    else{glColor3ub(41, 163, 41);}
        drawQuad(-2.0f, 1.0f, 4.0f, 2.0f);

    river(type);

    // Objects trees
        // Draw multiple trees using translations
        int g = 150;
        if(type == 2){
            g = 100;}
    glPushMatrix();
        glTranslatef(-1.5f, 0.55f, 0.0f);
        tree(1,0, g, 5); // Circle tree 1
    glPopMatrix();

    glPushMatrix();
        glTranslatef(-0.8f, 0.57f, 0.0f);
        tree(1,0, g, 5); // Circle tree 2
    glPopMatrix();

    glPushMatrix();
        glTranslatef(-0.2f, 0.65f, 0.0f);
        tree(2,0, g, 5); // Triangle tree 1
    glPopMatrix();
//tree end

	road_footpath();

    glPushMatrix();
    glTranslatef(0.9, position_c3, 0.0f);
    glRotatef(90, 0.0f, 0.0f, 1.0f);
    car();
    glPopMatrix();

    glPushMatrix();
    glTranslatef(0.9f, position_c4, 0.0f);
    glRotatef(270 ,0.0f, 0.0f, 1.0f);
    car();
    glPopMatrix();

	traffic_light1();
	traffic_light2();
    road_light(type);

    shop();
    if (type == 2){
        glBegin(GL_POLYGON); // door
        glColor3ub(255, 153, 51);
        glVertex2f(-1.9f, 0.5f);
        glVertex2f(-1.9f, 0.62f);
        glColor3ub(179, 119, 0);
        glVertex2f(-1.82f, 0.62f);
        glVertex2f(-1.82f, 0.5f);
    glEnd();

    glBegin(GL_POLYGON); // window
        glColor3ub(255, 153, 51);
        glVertex2f(-1.77f, 0.55f);
        glVertex2f(-1.77f, 0.62f);
        glColor3ub(179, 119, 0);
        glVertex2f(-1.6f, 0.62f);
        glVertex2f(-1.6f, 0.55f);
    glEnd();

// shutter
        glColor3ub(102, 82, 0);
        drawQuad(-0.73f, 0.8, 0.51f, 0.3f);


    glBegin(GL_POLYGON); // shelter
        glColor3ub(230, 115, 0);
        glVertex2f(-0.8f, 0.7f);
        glVertex2f(-0.15f, 0.7f);
        glVertex2f(-0.15f, 0.75f);
        glVertex2f(-0.2f, 0.8f);
        glVertex2f(-0.75f, 0.8f);
        glVertex2f(-0.8f, 0.75f);
    glEnd();

        glColor3ub(128, 0, 0);
        drawLine(-0.17f, 0.5f,-0.77f, 0.5f);
        drawLine(-0.17f, 0.504f,-0.77f, 0.504f);

        glColor3ub(255, 153, 51);
        drawLine(-0.73f, 0.53f,-0.22f, 0.53f);
        drawLine(-0.73f, 0.56f,-0.22f, 0.56f);
        drawLine(-0.73f, 0.59f,-0.22f, 0.59f);
        drawLine(-0.73f, 0.62f,-0.22f, 0.62f);
        drawLine(-0.73f, 0.65f,-0.22f, 0.65f);
        drawLine(-0.73f, 0.68f,-0.22f, 0.68f);


        glColor3ub(255, 235, 153);
        drawQuad(-0.4f, 0.65f, 0.15f, 0.07f);
        glColor3ub(102, 51, 0);
    glRasterPos2f(-0.39, 0.6);
    for(int i = 0; c[i] !='\0'; i++)
        glutBitmapCharacter(GLUT_BITMAP_8_BY_13, c[i]);
        house(63, 72, 204);
    }else{    house(179, 230, 255);
}


    // shop open
    glColor3ub(255, 255, 204);
    drawQuad(-1.72f, 0.6f, 0.07f, 0.03f);

    glColor3ub(255, 51, 0);
    glRasterPos2f(-1.715 , 0.58);
    for(int i = 0; c[i] !='\0'; i++)
        glutBitmapCharacter(GLUT_BITMAP_HELVETICA_10, c[i]);
    // shop open


    glPushMatrix();
    // Translate to the bottom-left corner
    glScalef(0.008,0.004,0.01);
    glTranslatef(-130, -150, 0);
    fullTheater();
    glPopMatrix();

    if(type == 1){
        glPushMatrix();
    // Translate to the bottom-left corner
    glScalef(0.008,0.004,0.01);
    glColor3f(0.7, 0.0, 0.0);
    drawQuad(-210, 50, 170, 140);
    glPopMatrix();
    }

    drawFish(1.51, 0.3);
    drawFish(1.65, -0.22);
    drawFish(1.85, -0.18);
    drawFish(1.61, -0.45);
    drawFish(1.91, 0.1);

    glFlush();
}

 void day(){

 time(1);
 }
 void night(){
 time(2);
 }

void button(unsigned char key, int x, int y) {


    switch (key) {
        case 'f':
            glutDisplayFunc(day);
            break;

        case 'r':
            cnt++;
            break;

        case 's':
            cnt = 0;
            break;

        case 'n':
            flag++;
            glutDisplayFunc(night);
            glutPostRedisplay();
            break;

        case 'd':
            flag = 0;
            glutDisplayFunc(day);
            glutPostRedisplay();
            break;
    }
}

void update_fish(int value) {
    if (cnt == 0) {
        if (position_fish > -0.9) {
            speed_fish = 0.0f;
            position_fish = -0.9;
        }
        position_fish += speed_fish;
    } else {
        speed_fish = 0.0007f;
        if (position_fish > 1.7)
            position_fish = -1.7f;
        position_fish += speed_fish;
    }
    glutPostRedisplay();
    glutTimerFunc(10, update_fish, 0);
}

void update_car3(int value) {
    if (cnt == 0) {
        if (position_c3 > -0.9) {
            speed_c3 = 0.0f;
            position_c3 = -0.9;
        }
        position_c3 += speed_c3;
    } else {
        speed_c3 = 0.01f;
        if (position_c3 > 1.7)
            position_c3 = -1.7f;
        position_c3 += speed_c3;
    }
    glutPostRedisplay();
    glutTimerFunc(10, update_car3, 0);
}

void update_car4(int value) {
    if (cnt == 0) {
        if (position_c4 < 0.9) {
            speed_c4 = 0.0f;
            position_c4 = 0.9;
        }
        position_c4 -= speed_c4;
    } else {
        speed_c4 = 0.01f;
        if (position_c4 < -1.7)
            position_c4 = 1.7f;
        position_c4 -= speed_c4;
    }
    glutPostRedisplay();
    glutTimerFunc(10, update_car4, 0);
}

void inigl() {
    glClearColor(1.0f, 1.0f, 1.0f, 1.0f); // Set background color to white
    gluOrtho2D(-2, 2, -1, 1); // Set range of axis of display
}

int main(int argc, char** argv) {
    glutInit(&argc, argv); // Initialize GLUT
    glutInitDisplayMode(GLUT_SINGLE | GLUT_RGB);
    glutInitWindowSize(1600, 800); // Set the window's initial width & height
    glutInitWindowPosition(0, 0); // Set the window position
    glutCreateWindow("City"); // Create a window with the given title
    glutDisplayFunc(start);
    inigl();
    glutTimerFunc(10, update_fish, 0);
    glutTimerFunc(10, update_car3, 0);
    glutTimerFunc(10, update_car4, 0);
    glutKeyboardFunc(button);
    glutMainLoop();
    return 0;
}
