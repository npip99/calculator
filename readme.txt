-----How to use my framework, extension of Chili framework-----
- Overview
In Main.h there are two areas designated for user input. One area is where your functions will go, and the
other will be for variables. Variables will be intialized in the constructor ( Game::Game( HWND, HINSTANCE, Input ); ).
The constructor will only run once. UpdateFrame() is looped 60 times per second until the player hits the red 'X'.
After that the deconstructor will be called ( Game::~Game() ). The deconstructor will only run once before exiting.
You can look at the other files, but only Main.cpp and Main.h are necessary. I have left comments in the other files.

- Rendering pixels
In order to draw a pixel on the screen, you must put gfx.DrawPixel( x, y, D3DCOlOR_XRGB( r, g, b ) ); 
D3DCOLOR is a macro for color on a rating from 0 to 255 for red, green, and blue. All 255's will be white. All 0's will be black. 
Note: The origin is in the top left corner, and the Y axis is flipped ( higher numbers going down ).

- Rendering Lines
I have provided you with a line function that uses y = mx + b. You can look through the file and try to reverse engineer it.
To use the function, write gfx.DrawLine( x1, y1, x2, y2, D3DCOLOR_XRGB( r, g, b ) ); X1 and Y1 are the coordinates for the
first point, while X2 and Y2 define the second point. D3DCOLOR has been explained.

- Rendering Sprites
You can import bitmap files for an easier and more efficient way of loading sprites. There are several steps to importing
sprites. First, in Main.h, you must make a D3DCOLOR array. The array MUST be the dimensions of the file ( EX: D3DCOLOR Sprite1Texture[ 32*32 ] ).
You must also create a 'Sprite' struct (located in Graphics.cpp) in Main.h ( EX: Sprite Sprite1; ). In order to load the sprites, you must call
LoadSprite( const char* filename, Sprite* sprite, D3DCOLOR* Surface, D3DCOLOR key ); filename is where the file is located ( EX: "AppFiles//Sprite.bmp" ).
sprite is a pointer to the sprite ( EX: &Sprite1 ). Surface is the D3DCOLOR array ( EX: &Sprite1Texture ). Key is the transparent color, which is
almost always a shade of magenta ( EX: D3DCOLOR_XRGB( 255,0,255 +) ). When you finish, it is easy to render them, all you have to do is call a function.
( EX: LoadSprite( "Sprite.bmp", &Sprite1, Sprite1Texture, D3DCOLOR_XRGB( 255,0,255 ) ); ) --- Finished Product ---
In order to render them, you must use gfx.DrawSprite( float xoffset, float yoffset, Sprite* sprite, D3DCOLOR c ). x and y offsets are the coordinates of
the sprite. The coordinates are referred to as the top left corner of the sprite. sprite is the address of the sprite ( EX: &Sprite ). c is the color
you would like to produce it in ( Use NULL to signify the colors in the bitmap file ).
( EX: gfx.DrawSprite( 1, 1, &Sprite1, NULL ); ) --- Finished Product ---

- Rendering Text
You can also import text in a single bitmap file, rather than several seperate bitmap files. You must create a D3DCOLOR array, like rendering sprites.
There is also a Font struct, much like a sprite ( EX: Font font1; ). To load the font, you must create a monospaced bitmap in ASCII. ( See Example .bmp ).
Here is the prototype: LoadFont( const char* filename, Font* font, D3DCOLOR* Surface, int CharWidth, int CharHeight, int CharsPerRow ); --- Graphics.h
( EX: LoadFont( "Font.bmp", &font1, &FontTexture, 16, 28, 36 );
To render the text, you must use the address of the font ( EX: &font1 ). gfx.DrawChar( char Chars, float x, float y, Font* font, D3DCOLOR c ); is used
to render a single character, where x and y is the top left of the character. ( EX: gfx.DrawChar( 'a', 1, 1, &font1, D3DCOLOR_XRGB( 255,255,255 ) );
NOTE: As of right now, the framework only supports black and white fonts!
gfx.DrawString(); works in the same way. ( EX: gfx.DrawString( "Hello World!", 500, 500, &font1, D3DCOLOR_XRGB( 255,255,255 ) );

- DirectInput
A video game, by definition, requires input. Without input, it is just a movie or cinimatic. In order to get input, you should use DirectInput. The Windows
API comes with certain input structures, but it is extremely inefficient when dealing with video games. The DirectInput class (defined in Input.cpp) has two
ways of collecting data. You can check the key state using CheckKey(int) function. You may use the Keyboard_Device typedef enumeration for the keys. ( msdn.com )
( EX: DIK_LEFT ) There is also a DIMOUSESTATE structure (named mouse). In DirectInput, you will not get a coordinate for the mouse, you will get raw mouse data. 
In the first frame, the DINMOUSESTATE's lX and lY and lZ (the wheel) will be set to zero. In the 1/60th of a second it takes to render the frame, the mouse will
calculate how many milimeters the mouse moved. The amount it moved in the last 1/60th of a second will be located in the lX and lY members. This number will
usually be somewhere in between 20-30 if the mouse is moving, depending on the distance it moved. When the mouse is moving left or up, than the respective X and Y
value will be negative. This allows an easy way to render your own cursor, over importing a custom *.cur and letting windows handle the cursor. In the
DINMOUSESTATE struct you will also have a BYTE array named 'rgbButtons'. [0] is left click, [1] is right click, [2] is middle click ( [3] is any other button ).



  Good luck programming!
    ~Nick