prontera,150,150,0	script	IllusionMaster 123,{
	OnInit:
		function createAndMoveUnit;
		//constants
		.FIELD_HEIGHT = 16;
		.FIELD_WIDTH = 16;
		getmapxy(.CURRENT_MAP$, .FIELD_CENTER_X, .FIELD_CENTER_Y, UNITTYPE_NPC, strnpcinfo(0));
		.FIELD_CENTER_Y -= .FIELD_HEIGHT;
		.RIGHT = 0;
		.DOWN = 1;
		.LEFT = 2;
		.UP = 3;
		end;

	initnpctimer;

	OnTimer3000:
		createAndMoveUnit(.DOWN, -5);    
		end;
	OnTimer6500:
		createAndMoveUnit(.DOWN, 3);    
		end;
	OnTimer105000:
		createAndMoveUnit(.RIGHT, 0, 4);    
		end;
	OnTimer14000:
		createAndMoveSimpleWall(.RIGHT, -6, 6);    
		end;
	OnTimer15500:
		createAndMoveSimpleWall(.UP, -2, 5);    
		end;        

	stopnpctimer;


	/* Function definition */
	function	createAndMoveUnit	{
		//constants
		.@DEFAULT_MONSTER_ID = 1002;
		.@DEFAULT_NAME$ = "Unit";

		.@direction = getarg(0);
		.@startCoord = getarg(1);
		.@name$ = getargcount() > 2 ? getarg(2) : .@DEFAULT_NAME$;
		.@monsterId = getargcount() > 3 ? getarg(3) : .@DEFAULT_MONSTER_ID;

		.@startX = .FIELD_CENTER_X;
		.@startY = .FIELD_CENTER_Y;
		.@endX = .FIELD_CENTER_X;
		.@endY = .FIELD_CENTER_Y;

		switch( .@direction ) {
		   case 0: //RIGHT
			   .@startX -=  .FIELD_WIDTH;  
			   .@startY += .@startCoord; 
			   .@endX += .FIELD_WIDTH;   
			   .@endY = .@startY;
		   end; 
		   case 1: //DOWN
			   .@startX += .@startCoord;
			   .@startY += .FIELD_HEIGHT; 
			   .@endX = .@startX;
			   .@endY -= .FIELD_HEIGHT;   
		   end; 
		   case 2: //LEFT
			   .@startX +=  .FIELD_WIDTH;  
			   .@startY += .@startCoord; 
			   .@endX -= .FIELD_WIDTH;   
			   .@endY = .@startY;
		   end; 
		   case 3: //UP
			   .@startX += .@startCoord;
			   .@startY -= .FIELD_HEIGHT; 
			   .@endX = .@startX;
			   .@endY += .FIELD_HEIGHT; 
		   end; 
		}

		monster .CURRENT_MAP$,.@startX,.@startY,.@name$,.@monsterId,1;
		.@gid = $@mobid[0];
		unitwalk .@gid,.@endX,.@endY;
		unitkill .@gid;
		return .@gid;
	}

	function	createAndMoveSimpleWall	{
		//constants
		.@DEFAULT_MONSTER_ID = 1002;
		.@DEFAULT_NAME$ = "Unit";

		.@direction = getarg(0);
		.@startCoord = getarg(1);
		.@lenght = getarg(2);
		.@name$ = getargcount() > 3 ? getarg(3) : .@DEFAULT_NAME;
		.@monsterId = getargcount() > 4 ? getarg(4) : .@DEFAULT_MONSTER_ID

		switch( .@direction ) {
		   case 0: //RIGHT
			   .@startX -= .FIELD_WIDTH;  
			   .@startY += .@startCoord; 
			   .@endX += .FIELD_WIDTH;   
			   .@endY = .@startY;
			   for ( .@monsterNumber = 0; .@monsterNumber < .@lenght; .@monsterNumber++){
					createAndMoveUnit(.@direction, .@startY + .@monsterNumber, .@name$, .@monsterId);
				}		
		   end; 
		   case 1: //DOWN
			   .@startX += .@startCoord;
			   .@startY += .FIELD_HEIGHT; 
			   .@endX = .@startX;
			   .@endY -= .FIELD_HEIGHT;   
			   for ( .@monsterNumber = 0; .@monsterNumber < .@lenght; .@monsterNumber++){
					createAndMoveUnit(.@direction, .@startX + .@monsterNumber, .@name$, .@monsterId);
				}		
		   end; 
		   case 2: //LEFT
			   .@startX +=  .FIELD_WIDTH;  
			   .@startY += .@startCoord; 
			   .@endX -= .FIELD_WIDTH;   
			   .@endY = .@startY;
			   for ( .@monsterNumber = 0; .@monsterNumber < .@lenght; .@monsterNumber++){
					createAndMoveUnit(.@direction, .@startY + .@monsterNumber, .@name$, .@monsterId);
				}	
		   end; 
		   case 3: //UP
			   .@startX += .@startCoord;
			   .@startY -= .FIELD_HEIGHT; 
			   .@endX = .@startX;
			   .@endY += .FIELD_HEIGHT; 
			   	for ( .@monsterNumber = 0; .@monsterNumber < .@lenght; .@monsterNumber++){
					createAndMoveUnit(.@direction, .@startX + .@monsterNumber, .@name$, .@monsterId);
				}	
		   end; 
		}
	}
}

function    script    rotateMonsters    {    
}