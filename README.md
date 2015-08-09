#include "SFML/Graphics.hpp"
#include "SFML/Audio.hpp"
#include <iostream>
#include <string>
using namespace std;
using namespace sf;
 
/*
Twitter.com/LiottaFabrizio
Twitter.com/LiottaFabrizio
Twitter.com/LiottaFabrizio
Twitter.com/LiottaFabrizio
Twitter.com/LiottaFabrizio
*/
Event event;
Sprite personaje[2];
Texture personaje_t;
Sprite background;
Texture background_t;
Sprite bola;
Texture bola_t;
Text Puntos_P1;
Text Puntos_P2;
Font fuente;
Sound pelota;
SoundBuffer pelota_s;
Sound perder;
SoundBuffer perder_s;
float velocidad;
bool juego = false;
int Score_P1 = 0 , Score_P2 = 0;
int Movimiento_y = 1,Movimiento_x = 1;
int players = 1;
 
void movimiento();
void movimiento_bot();
void bola_m();
void score();
int main(){
        RenderWindow window(VideoMode(1024, 725), "PONG -WinakoTF");
        background_t.loadFromFile("fondo.png");
        background.setTexture(background_t);
        personaje_t.loadFromFile("barrita.png");
        personaje[0].setTexture(personaje_t);
        personaje[1].setTexture(personaje_t);
        bola_t.loadFromFile("bola.png");
        bola.setTexture(bola_t);
        fuente.loadFromFile("fuente.ttf");
        Puntos_P1.setFont(fuente);
        Puntos_P2.setFont(fuente);
        pelota_s.loadFromFile("Pelota.wav");
        perder_s.loadFromFile("perder.wav");
        pelota.setBuffer(pelota_s);
        perder.setBuffer(perder_s);
        personaje[0].setPosition(12, 300);
        personaje[1].setPosition(990, 300);
        bola.setPosition(410, 340);
        while (window.isOpen())
        {
                while (window.pollEvent(event))
                {
                        switch (event.type)
                        {
                        case Event::Closed:
                                window.close();
                        }
                }
                if (juego){
                movimiento();
                if (players == 1){ movimiento_bot(); }
                bola_m();
                score();
                }
                else if (!juego){
                        if (Keyboard::isKeyPressed(Keyboard::W) || Keyboard::isKeyPressed(Keyboard::Up)){
                                bola.setPosition(410, 340);
                                pelota.play();
                                players = 1;
                        }
                        else if (Keyboard::isKeyPressed(Keyboard::S) || Keyboard::isKeyPressed(Keyboard::Down)){
                                bola.setPosition(410, 410);
                                pelota.play();
                                players = 2;
                        }
                        if (Keyboard::isKeyPressed(Keyboard::Return)){
                                juego = 1;
                                bola.setPosition(504, 600);
                                background_t.loadFromFile("fondo2.png");
                        }
 
                }
       
                window.clear();
                window.draw(background);
                window.draw(personaje[0]);
                window.draw(personaje[1]);
                window.draw(Puntos_P1);
                window.draw(Puntos_P2);
                window.draw(bola);
                window.display();
        }
}
 
void movimiento(){
        if (Keyboard::isKeyPressed(Keyboard::W)){
                if (personaje[0].getPosition().y > 5){
                personaje[0].move(0, -1);
                }
        }
        if (Keyboard::isKeyPressed(Keyboard::S)){
                if (personaje[0].getPosition().y < 605){
                        personaje[0].move(0, 1);
                }
        }
        if (players == 2)
        {
        if (Keyboard::isKeyPressed(Keyboard::Up)){
                if (personaje[1].getPosition().y > 5){
                        personaje[1].move(0, -1);
                }
        }
        if (Keyboard::isKeyPressed(Keyboard::Down)){
                if (personaje[1].getPosition().y < 605){
                        personaje[1].move(0, 1);
                }
        }
        }
}
void movimiento_bot(){
 
        if (personaje[1].getPosition().y < bola.getPosition().y){
                personaje[1].move(0, 0.96f);
        }
        else if (personaje[1].getPosition().y > bola.getPosition().y){
                personaje[1].move(0, -0.96f);
        }
}
void bola_m(){
        if (bola.getPosition().y < 0){
                pelota.play();
                Movimiento_y = 1;
        }
        else if (bola.getPosition().y > 720){
                pelota.play();
                Movimiento_y = 2;
        }
 
        if (Movimiento_y == 1){
                bola.move(0, 1);
        }
        else if (Movimiento_y == 2){
                bola.move(0, -1);
        }
 
        if (bola.getGlobalBounds().intersects(personaje[0].getGlobalBounds())){
                Movimiento_x = 1;
                pelota.play();
        }
        else if (bola.getGlobalBounds().intersects(personaje[1].getGlobalBounds())){
                Movimiento_x = 2;
                pelota.play();
        }
 
        if (Movimiento_x == 1){
                bola.move(1, 0);
        }
        else if (Movimiento_x == 2){
                bola.move(-1, 0);
        }
}
void score(){
        int a = 0;
        if (bola.getPosition().x < 0){
                perder.play();
                Score_P2++;    
                bola.setPosition(504, 600);
               
        }
        else if (bola.getPosition().x > 1000){
                Score_P1++;
                perder.play();
 
                bola.setPosition(504, 600);
        }
 
        Puntos_P1.setString(" " + std::to_string(Score_P1));
        Puntos_P2.setString(" " + std::to_string(Score_P2));
        Puntos_P1.setColor(Color::White);
        Puntos_P1.setPosition(300, 150);
        Puntos_P1.setScale(3, 3);
        Puntos_P2.setPosition(600, 150);
        Puntos_P2.setScale(3, 3);
}
