0H W0 YY FF UU UU UU UU BB X1 X1 X2 X2 X3 X3 X4 X4 X5 X5 X6 X6 X7 X7 X8 X8 X9 X9 XA XA XB XB XC XC XD XD XE XE XF XF XG XG

U - Unknown
Y - Unknown, has something to do with height, very strange
H - Height
W - Width
B - Bank, i.e. first byte in SNES address of images
F - Flag byte, it's done in binary, and I'm not exactly sure how it works
		UUUPPPUU
	P - palette to use for all sprites in the collection


X1 - First north facing sprite
X2 - Second north facing sprite
X3 - First east facing sprite
X4 - Second east facing sprite
X5 - First south facing sprite
X6 - Second south facing sprite
X7 - First west facing sprite
X8 - second west facing sprite
X9 - First north-east facing sprite
XA - Second north-east facing sprite
XB - First south-east facing sprite
XC - Second south-east facing sprite
XD - First south-west facing sprite
XE - Second south-west facing sprite
XF - First north-west facing sprite
XG - Second north-west facing sprite

Note: To get a mirror of desired image, add one to the address.
Note 2: There is nothing that signifies that the diagonal sprites are not used. They simply are not accessed by the usual
use of the sprite. Only the chosen four and some NPC's have diagonal sprites.