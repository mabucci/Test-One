/*****************************************************************************

  		  |	                 |
	 G0	  |       G1         |      G2
--------|------------------|--------------- Y0
  		  |		               |
	 G3   |       G4         |      G5
	  	  |		               |
--------|------------------|--------------- Y1
	 G6   |        G7        |       G8
	     X0                   X1
G0, G2, G3, G5, G6, and G8 are out of any axis confine
G3, G5, G6, and G8 are in a single axis confine
G4 is in a double axis confine
*******************************************************************************/
#include <cmath> // for sqrt() function
#include <iostream>
#include <iomanip>
#include <limits>
#include <string>


void calculateGamesMove(int, std::string, int[], int[][2], int[], int&, double[]);


int main()
{
	int Goal[3][2]{ {5,10}, {5, 10},{5, 10} };
	int Game[3]{ 0 , 0, 0 };
	int DeltaXYZ[3]{ 0,0,0 };
	int DeltaXYZTotal{ 0 };
	double DeltaPercent[3]{ 0, 0, 0 };
	double DeltaPercentTotal{ 0 };
	char AxisLables[3]{ 'X', 'Y', 'Z' };
	int GoalTarget[3][2]{ {0, 0}, {0, 0}, {0, 0} };
	int SectorNumber{ 0 };
	std::string saAxisConfineData[3]{ "\0","\0","\0" };  // [0] holds axis confine, [1] holds sector GAME is in, and [2] hold GAME's position relationship to GOAL's Z axis planes
	std::string saAxisConfineTitles[6]{ "Out of any axis confine.", "In single asix confine.", "In double axis confine.", "<- Goal[2][0]",\
"[Goal[2][0] Goal[2][1]]", "-> Goal[2][1]" };
	std::string saAxisSector[9]{ "G0", "G1", "G2", "G3", "G4", "G5", "G6", "G7", "G8" };

	while (1)
	{
		/*************/
		std::cout << '\n' << "Enter Game's X: ";
		std::cin >> Game[0];
		std::cout << "Enter Game's Y: ";
		std::cin >> Game[1];
		std::cout << "Enter Game's Z: ";
		std::cin >> Game[2];
		/**********************/
		// G0, G2, G6, and G8 are out of any axis confine  ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
		if ((Game[0] < Goal[0][0] && (Game[1]<Goal[1][0] || Game[1]>Goal[1][1])) ||
			(Game[0] > Goal[0][1] && (Game[1]<Goal[1][0] || Game[1]>Goal[1][1])))
		{
			saAxisConfineData[0] = saAxisConfineTitles[0];
			if (Goal[0][0] - Game[0] > 0 && Goal[1][0] - Game[1] > 0) // GAME is in G0
			{
				saAxisConfineData[1] = saAxisSector[0];
				SectorNumber = 0;
				if (Goal[2][0] > Game[2]) saAxisConfineData[2] = saAxisConfineTitles[3];
				else if (Goal[2][0] <= Game[2] && Game[2] <= Goal[2][1])  saAxisConfineData[2] = saAxisConfineTitles[4];
				else if (Goal[2][1] < Game[2])  saAxisConfineData[2] = saAxisConfineTitles[5];
			}
			else if (Goal[0][1] - Game[0] < 0 && Goal[1][1] - Game[1] > 0) // GAME is in G2
			{
				saAxisConfineData[1] = saAxisSector[2];
				SectorNumber = 2;
				if (Goal[2][0] > Game[2]) saAxisConfineData[2] = saAxisConfineTitles[3];
				else if (Goal[2][0] <= Game[2] && Game[2] <= Goal[2][1])  saAxisConfineData[2] = saAxisConfineTitles[4];
				else if (Goal[2][1] < Game[2])  saAxisConfineData[2] = saAxisConfineTitles[5];
			}
			else if (Goal[0][0] - Game[0] > 0 && Goal[1][1] - Game[1] < 0) // GAME is in G6
			{
				saAxisConfineData[1] = saAxisSector[6];
				SectorNumber = 6;
				if (Goal[2][0] > Game[2]) saAxisConfineData[2] = saAxisConfineTitles[3];
				else if (Goal[2][0] <= Game[2] && Game[2] <= Goal[2][1])  saAxisConfineData[2] = saAxisConfineTitles[4];
				else if (Goal[2][1] < Game[2])  saAxisConfineData[2] = saAxisConfineTitles[5];
			}
			else if (Goal[0][1] - Game[0] < 0 && Goal[1][1] - Game[1] < 0) // GAME is in G8
			{
				saAxisConfineData[1] = saAxisSector[8];
				SectorNumber = 8;
				if (Goal[2][0] > Game[2]) saAxisConfineData[2] = saAxisConfineTitles[3];
				else if (Goal[2][0] <= Game[2] && Game[2] <= Goal[2][1])  saAxisConfineData[2] = saAxisConfineTitles[4];
				else if (Goal[2][1] < Game[2])  saAxisConfineData[2] = saAxisConfineTitles[5];
			}
		} // end if on << "Out of any axis confine." -----------------------------------------------------------------

	// G1, G3, G5, and G7 are in a single axis confine +++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
		if ((Game[0] <= Goal[0][0] && Game[1] >= Goal[1][0] && Game[1] <= Goal[1][1]) || (Game[0] >= Goal[0][1] &&
			Game[1] >= Goal[1][0] && Game[1] <= Goal[1][1]) || (Game[0] >= Goal[0][0] && Game[0] <= Goal[0][1] &&
				Game[1] <= Goal[1][0]) || (Game[0] >= Goal[0][0] && Game[0] <= Goal[0][1] && Game[1] >= Goal[1][1]))
		{
			saAxisConfineData[0] = saAxisConfineTitles[1];
			if (Game[0] >= Goal[0][0] && Game[0] <= Goal[0][1] && Game[1] <= Goal[1][0]) // GAME is in G1
			{
				saAxisConfineData[1] = saAxisSector[1];
				SectorNumber = 1;
				if (Goal[2][0] > Game[2]) saAxisConfineData[2] = saAxisConfineTitles[3];
				else if (Goal[2][0] <= Game[2] && Game[2] <= Goal[2][1])  saAxisConfineData[2] = saAxisConfineTitles[4];
				else if (Goal[2][1] < Game[2])  saAxisConfineData[2] = saAxisConfineTitles[5];
			}
			else  if (Game[0] <= Goal[0][0] && Game[1] >= Goal[1][0] && Game[1] <= Goal[1][1]) // GAME is in G3
			{
				saAxisConfineData[1] = saAxisSector[3];
				SectorNumber = 3;
				if (Goal[2][0] > Game[2]) saAxisConfineData[2] = saAxisConfineTitles[3];
				else if (Goal[2][0] <= Game[2] && Game[2] <= Goal[2][1])  saAxisConfineData[2] = saAxisConfineTitles[4];
				else if (Goal[2][1] < Game[2])  saAxisConfineData[2] = saAxisConfineTitles[5];
			}
			else if (Game[0] >= Goal[0][1] && Game[1] >= Goal[1][0] && Game[1] <= Goal[1][1]) // GAME is in G5
			{
				saAxisConfineData[1] = saAxisSector[5];
				SectorNumber = 5;
				if (Goal[2][0] > Game[2]) saAxisConfineData[2] = saAxisConfineTitles[3];
				else if (Goal[2][0] <= Game[2] && Game[2] <= Goal[2][1])  saAxisConfineData[2] = saAxisConfineTitles[4];
				else if (Goal[2][1] < Game[2])  saAxisConfineData[2] = saAxisConfineTitles[5];
			}
			else  if (Game[0] >= Goal[0][0] && Game[0] <= Goal[0][1] && Game[1] >= Goal[1][1]) // GAME is in G7 
			{
				saAxisConfineData[1] = saAxisSector[7];
				SectorNumber = 7;
				if (Goal[2][0] > Game[2]) saAxisConfineData[2] = saAxisConfineTitles[3];
				else if (Goal[2][0] <= Game[2] && Game[2] <= Goal[2][1])  saAxisConfineData[2] = saAxisConfineTitles[4];
				else if (Goal[2][1] < Game[2])  saAxisConfineData[2] = saAxisConfineTitles[5];
			}
		}  // end if on "In single asix confine." -------------------------------------------------------------------------

		//  G4 is in a double axis confine ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
		if ((Game[0] >= Goal[0][0] && Game[0] <= Goal[0][1] && Game[1] >= Goal[1][0] && Game[1] <= Goal[1][1]))
		{
			saAxisConfineData[0] = saAxisConfineTitles[2];
			saAxisConfineData[1] = saAxisSector[4];
			SectorNumber = 4;
			if (Goal[2][0] > Game[2]) saAxisConfineData[2] = saAxisConfineTitles[3];
			else if (Goal[2][0] <= Game[2] && Game[2] <= Goal[2][1])  saAxisConfineData[2] = saAxisConfineTitles[4];
			else if (Goal[2][1] < Game[2])  saAxisConfineData[2] = saAxisConfineTitles[5];
		}  // end f on  "In double axis confine." -------------------------------------------------------------------------- 

		std::cout << '\n' << saAxisConfineData[0];    // [0] holds axis confine, "Out of any axis confine." ....
		std::cout << '\n' << saAxisConfineData[1];    // [1] holds sector GAME is in, "G0", "G1" .......
		std::cout << '\n' << saAxisConfineData[2];    // [2] hold GAME's position relationship to GOAL's Z axis planes  "-> Goal[2][1]"....
		/*****
		for (variable : collection) {
		// code block
		}
		std::string saAxisSector[9]{ "G0", "G1", "G2", "G3", "G4", "G5", "G6", "G7", "G8" };
		*****/

		for (std::string s : saAxisSector)
		{
			if (s == saAxisConfineData[1])
			{
				std::cout << "\n\nIn Sector: " << s << " *****\n\n";
				break;
			}
		}

		// need to find delta to GOAL and delta percent to GOAL 
		calculateGamesMove(SectorNumber, saAxisConfineData[2], DeltaXYZ, Goal, Game, DeltaXYZTotal, DeltaPercent);


	for (int i{ 0 }; i < 3; ++i)
	{
		std::cout << "\nDelta " << AxisLables[i] << " Percent " << DeltaPercent[i]*100.0<<'%';
		DeltaPercentTotal += std::abs(DeltaPercent[i])*100.0;
		DeltaPercent[i] = 0;
	}
	std::cout << "\nDelta % Total ABS " << DeltaPercentTotal<<'%'<<'\n';
	DeltaPercentTotal = 0;

	for (int i{ 0 }; i < 3; ++i)
	{
		std::cout << "\nDelta " << AxisLables[i] << " distance to Goal " << DeltaXYZ[i] << " game space units.";
		DeltaXYZ[i] = 0;
	}

	std::cout << "\nDeltaXYZTotal ABS " << DeltaXYZTotal<<'\n';
	DeltaXYZTotal = 0;

		} // end 'while' 
		return 0;
};// END main main ENDS -------------------------------------------------------------------------------------------------------------
// ----------------------------------------------------------------------------------------------------------------------------------

// ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
// START calculateGamesMove calculateGamesMove STARTS +++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
void calculateGamesMove(int SectorNumber, std::string saAxisConfineData, int DeltaXYZ[], int Goal[][2], int Game[], int& DeltaXYZTotal, double DeltaPercent[])
{
	/*****
		int Goal[3][2]{ {5,10}, {5, 10},{5, 10} };
		int Game[3]{ 0 , 0, 0 };
		int DeltaXYZ[3]{ -1,-1,-1};
		double DeltaPercent[3]{ -1, -1, -1};

XYZ? == G0, G2, G6, G8
YZ?  == G1, G7
XZ?  == G3, G5
Z    == G4
*****/
	if (SectorNumber == 0 || SectorNumber == 2 || SectorNumber == 6 || SectorNumber == 8)
	{
		switch (SectorNumber)
		{
		case 0: { // +++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++ 'case 0'
			if (saAxisConfineData == "<- Goal[2][0]") // GAME's Z coordinate is below GOAL Z0 plane
			{
				for (int i{ 0 }; i < 3; ++i)
				{
					DeltaXYZ[i] = Goal[i][0] - Game[i];
					DeltaXYZTotal += DeltaXYZ[i];
				}
				for (int i{ 0 }; i < 3; ++i)
					DeltaPercent[i] = static_cast<double>(DeltaXYZ[i]) / DeltaXYZTotal;
			}
			else if (saAxisConfineData == "[Goal[2][0] Goal[2][1]]") // GAME's Z coordinate is between GOAL's Z0 to Z1 planes 
			{
				for (int i{ 0 }; i < 2; ++i)
				{
					DeltaXYZ[i] = Goal[i][0] - Game[i];
					DeltaXYZTotal += DeltaXYZ[i];
				}
				for (int i{ 0 }; i < 2; ++i)
					DeltaPercent[i] = static_cast<double>(DeltaXYZ[i]) / DeltaXYZTotal;
				DeltaPercent[2] = 0;

			}
			else if (saAxisConfineData == "-> Goal[2][1]") // above the GOAL 
			{
				for (int i{ 0 }; i < 2; ++i)
				{
					DeltaXYZ[i] = Goal[i][0] - Game[i];
					DeltaXYZTotal += DeltaXYZ[i];
				}
				DeltaXYZ[2] = Goal[2][1] - Game[2];
				DeltaXYZTotal += std::abs(DeltaXYZ[2]);
				for (int i{ 0 }; i < 3; ++i)
					DeltaPercent[i] = static_cast<double>(DeltaXYZ[i]) / DeltaXYZTotal;
			}

			break;
		}// end 'case 0' --------------------------------------------------------------------------- 'case 0'

		case 2: { // +++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++ 'case 2'
			if (saAxisConfineData == "<- Goal[2][0]") // GAME's Z coordinate is below GOAL Z0 plane
			{
				for (int i{ 0 }; i < 3; ++i)
				{
					if (i == 0)
						DeltaXYZ[i] = Goal[i][1] - Game[i];
					else
						DeltaXYZ[i] = Goal[i][0] - Game[i];
					DeltaXYZTotal += std::abs(DeltaXYZ[i]);
				}
				for (int i{ 0 }; i < 3; ++i)
					DeltaPercent[i] = static_cast<double>(DeltaXYZ[i]) / DeltaXYZTotal;
			}
			else if (saAxisConfineData == "[Goal[2][0] Goal[2][1]]")  // GAME's Z coordinate is between GOAL's Z0 to Z1 planes 
			{
				for (int i{ 0 }; i < 2; ++i)
				{
					if (i == 0)
						DeltaXYZ[i] = Goal[i][1] - Game[i];
					else
						DeltaXYZ[i] = Goal[i][0] - Game[i];
					DeltaXYZTotal += std::abs(DeltaXYZ[i]);
				}
				for (int i{ 0 }; i < 2; ++i)
					DeltaPercent[i] = static_cast<double>(DeltaXYZ[i]) / DeltaXYZTotal;

			}
			else if (saAxisConfineData == "-> Goal[2][1]") // above the GOAL 
			{
				for (int i{ 0 }; i < 2; ++i)
				{
					if (i == 0)
						DeltaXYZ[i] = Goal[i][1] - Game[i];
					else
						DeltaXYZ[i] = Goal[i][0] - Game[i];
					DeltaXYZTotal += abs(DeltaXYZ[i]);
				}
				DeltaXYZ[2] = Goal[2][1] - Game[2];
				DeltaXYZTotal += std::abs(DeltaXYZ[2]);
				for (int i{ 0 }; i < 3; ++i)
					DeltaPercent[i] = static_cast<double>(DeltaXYZ[i]) / DeltaXYZTotal;
			}
			break;
		}// end 'case 2' -------------------------------------------------------------------- 'case 2'

		case 6:{ // +++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++ 'case 6'
			if (saAxisConfineData == "<- Goal[2][0]") // GAME's Z coordinate is below GOAL Z0 axis
			{
				for (int i{ 0 }; i < 3; ++i)
				{
					if (i == 0)
						DeltaXYZ[i] = Goal[i][0] - Game[i];
					else if(i==1)
						DeltaXYZ[i] = Goal[i][1] - Game[i];
					else if(i==2)
						DeltaXYZ[i] = Goal[i][0] - Game[i];
					DeltaXYZTotal += std::abs(DeltaXYZ[i]);
				}
				for (int i{ 0 }; i < 3; ++i)
					DeltaPercent[i] = static_cast<double>(DeltaXYZ[i]) / DeltaXYZTotal;
			}
			else if (saAxisConfineData == "[Goal[2][0] Goal[2][1]]")  // GAME's Z coordinate is between GOAL's Z0 to Z1 planes 
			{
				for (int i{ 0 }; i < 2; ++i)
				{
					if (i == 0)
						DeltaXYZ[i] = Goal[i][0] - Game[i];
					else if(i == 1)
						DeltaXYZ[i] = Goal[i][1] - Game[i];
					DeltaXYZTotal += std::abs(DeltaXYZ[i]);
				}
				for (int i{ 0 }; i < 2; ++i)
					DeltaPercent[i] = static_cast<double>(DeltaXYZ[i]) / DeltaXYZTotal;

			}
			else if (saAxisConfineData == "-> Goal[2][1]") // above the GOAL 
			{
				for (int i{ 0 }; i < 2; ++i)
				{
					if (i == 0)
						DeltaXYZ[i] = Goal[i][0] - Game[i];
					else if(i == 1)
						DeltaXYZ[i] = Goal[i][1] - Game[i];
					DeltaXYZTotal += abs(DeltaXYZ[i]);
				}
				DeltaXYZ[2] = Goal[2][1] - Game[2];
				DeltaXYZTotal += std::abs(DeltaXYZ[2]);
				for (int i{ 0 }; i < 3; ++i)
					DeltaPercent[i] = static_cast<double>(DeltaXYZ[i]) / DeltaXYZTotal;
			}
			break;
		}// end 'case 6' -------------------------------------------------------------------- 'case 6'
			break;

		case 8: { // ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++ 'case 8'
			if (saAxisConfineData == "<- Goal[2][0]") // GAME's Z coordinate is below GOAL Z0 plane
			{
				for (int i{ 0 }; i < 3; ++i)
				{
					if (i == 0)
						DeltaXYZ[i] = Goal[i][1] - Game[i];
					else if(i==1)
						DeltaXYZ[i] = Goal[i][1] - Game[i];
					else if(i==2)
						DeltaXYZ[i] = Goal[i][0] - Game[i];
					DeltaXYZTotal += std::abs(DeltaXYZ[i]);
				}
				for (int i{ 0 }; i < 3; ++i)
					DeltaPercent[i] = static_cast<double>(DeltaXYZ[i]) / DeltaXYZTotal;
			}
			else if (saAxisConfineData == "[Goal[2][0] Goal[2][1]]")  // GAME's Z coordinate is between GOAL's Z0 to Z1 planes 
			{
				for (int i{ 0 }; i < 2; ++i)
				{
					if (i == 0)
						DeltaXYZ[i] = Goal[i][1] - Game[i];
					else if(i == 1)
						DeltaXYZ[i] = Goal[i][1] - Game[i];
					DeltaXYZTotal += std::abs(DeltaXYZ[i]);
				}
				for (int i{ 0 }; i < 2; ++i)
					DeltaPercent[i] = static_cast<double>(DeltaXYZ[i]) / DeltaXYZTotal;

			}
			else if (saAxisConfineData == "-> Goal[2][1]") // above the GOAL 
			{
				for (int i{ 0 }; i < 2; ++i)
				{
					if (i == 0)
						DeltaXYZ[i] = Goal[i][1] - Game[i];
					else if(i == 1)
						DeltaXYZ[i] = Goal[i][1] - Game[i];
					DeltaXYZTotal += abs(DeltaXYZ[i]);
				}
				DeltaXYZ[2] = Goal[2][1] - Game[2];
				DeltaXYZTotal += std::abs(DeltaXYZ[2]);
				for (int i{ 0 }; i < 3; ++i)
					DeltaPercent[i] = static_cast<double>(DeltaXYZ[i]) / DeltaXYZTotal;
			}   
			break;
		  }// end 'case 8' -------------------------------------------------------------------- 'case 8'
		}  // end 'switch' on 'SectorNumber' --------------------------------------------------
	} // end 'if' on  (SectorNumber == 0 || SectorNumber == 2 || SectorNumber == 6 || SectorNumber == 8)

   // In single asix confine the X axis G1 or G7  ++++++++++++++++++++++++++++++++++++++++++++++
	if (SectorNumber == 1 || SectorNumber == 7)
	{
	  switch (SectorNumber)
	  {
	case 1: { // +++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++ 'case 1'
		if (saAxisConfineData == "<- Goal[2][0]") // GAME's Z coordinant is below GOAL Z0 plane
		{
		for (int i{ 0 }; i < 3; ++i)
		{
			if (i == 0)
			{
			  DeltaXYZ[i] = 0;
			  continue; // the GAME icon is between the two GOAL X axes so no change in GAME’s X position is needed 
			}
			else {
				DeltaXYZ[i] = Goal[i][0] - Game[i];  // *(int(*)[3]DeltaXYZ  put this in watch window to see all of array values 
				DeltaXYZTotal += std::abs(DeltaXYZ[i]); // remember that arrays are passed to funcions only as pointers 
			}  // that's why C++'s class system is esar to use  
			for (int i{ 0 }; i < 3; ++i)
				DeltaPercent[i] = static_cast<double>(DeltaXYZ[i]) / DeltaXYZTotal;
		}
		}else if (saAxisConfineData == "[Goal[2][0] Goal[2][1]]")  // GAME's Z coordinate is between GOAL's Z0 to Z1 planes 
		{
		 	for (int i{ 0 }; i < 2; ++i)
				{
					if (i == 0)
						DeltaXYZ[i] = 0;
					else if(i==1)
						DeltaXYZ[i] = Goal[i][0] - Game[i];
					else if(i==2)
					  DeltaXYZ[i] = 0;
					DeltaXYZTotal += std::abs(DeltaXYZ[i]);  
				}
			for (int i{ 0 }; i < 3; ++i)
				DeltaPercent[i] = static_cast<double>(DeltaXYZ[i]) / DeltaXYZTotal;
		}else if(saAxisConfineData == "-> Goal[2][1]") // GAME's Z coordinate is above GOAL's Z1 plane   
		  {
		    for (int i{ 0 }; i < 3; ++i)
		    {
			if (i == 0)
			{
			  DeltaXYZ[i] = 0;
			  continue; // the GAME icon is between the two GOAL X axes so no change in GAME’s X position is needed 
			}
			else if(i == 1){
				DeltaXYZ[i] = Goal[i][0] - Game[i];  // *(int(*)[3]DeltaXYZ  put this in watch window to see all of array values 
				DeltaXYZTotal += std::abs(DeltaXYZ[i]); // remember that arrays are passed to funcions only as pointers 
			}else if(i == 2)  // that's why C++'s class system is esar to use  
			{
			    DeltaXYZ[i] = Goal[i][1] - Game[i];  // *(int(*)[3]DeltaXYZ  put this in watch window to see all of array values 
				DeltaXYZTotal += std::abs(DeltaXYZ[i]); // remember that arrays are passed to funcions only as pointers 
			}
			for (int i{ 0 }; i < 3; ++i)
				DeltaPercent[i] = static_cast<double>(DeltaXYZ[i]) / DeltaXYZTotal;
		}
		  }
		break;  // *(<type>(*)[arrSize])arrName == *(int(*)[3][2])Goal
	   }// end 'case' 1 -------------------------------------------------------------------------- 'case' 1
	 
	case 7: { // +++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++ 'case 7'
		if (saAxisConfineData == "<- Goal[2][0]") // GAME's Z coordinant is below GOAL Z0 plane
		{
		for (int i{ 0 }; i < 3; ++i)
		{
			if (i == 0)
			{
			  DeltaXYZ[i] = 0;
			  continue; // the GAME icon is between the two GOAL X axes so no change in GAME’s X position is needed 
			}
			else{
				DeltaXYZ[i] = Goal[i][1] - Game[i];  // *(int(*)[3]DeltaXYZ  put this in watch window to see all of array values 
				DeltaXYZTotal += std::abs(DeltaXYZ[i]); // remember that arrays are passed to funcions only as pointers 
			}  // that's why C++'s class system is esar to use  
			for (int i{ 0 }; i < 3; ++i)
				DeltaPercent[i] = static_cast<double>(DeltaXYZ[i]) / DeltaXYZTotal;
		}
		}else if (saAxisConfineData == "[Goal[2][0] Goal[2][1]]")  // GAME's Z coordinate is between GOAL's Z0 to Z1 planes 
		{
		 	for (int i{ 0 }; i < 2; ++i)
				{
					if (i == 0)
						DeltaXYZ[i] = 0;
					else if(i==1)
						DeltaXYZ[i] = Goal[i][0] - Game[i];
					else if(i==2)
					  DeltaXYZ[i] = 0;
					DeltaXYZTotal += std::abs(DeltaXYZ[i]);  
				}
			for (int i{ 0 }; i < 3; ++i)
				DeltaPercent[i] = static_cast<double>(DeltaXYZ[i]) / DeltaXYZTotal;
		}else if(saAxisConfineData == "-> Goal[2][1]") // GAME's Z coordinate is above GOAL's Z1 plane   
		  {
		    for (int i{ 0 }; i < 3; ++i)
		    {
			if (i == 0)
			{
			  DeltaXYZ[i] = 0;
			  continue; // the GAME icon is between the two GOAL X axes so no change in GAME’s X position is needed 
			}
			else if(i == 1){
				DeltaXYZ[i] = Goal[i][1] - Game[i];  // *(int(*)[3]DeltaXYZ  put this in watch window to see all of array values 
				DeltaXYZTotal += std::abs(DeltaXYZ[i]); // remember that arrays are passed to funcions only as pointers 
			}
			else if(i == 2)  // that's why C++'s class system is esar to use  
			{
			    DeltaXYZ[i] = Goal[i][1] - Game[i];  // *(int(*)[3]DeltaXYZ  put this in watch window to see all of array values 
				DeltaXYZTotal += std::abs(DeltaXYZ[i]); // remember that arrays are passed to funcions only as pointers 
			} 
			for (int i{ 0 }; i < 3; ++i)
				DeltaPercent[i] = static_cast<double>(DeltaXYZ[i]) / DeltaXYZTotal;
		}
		  }
		break;
		}
       }// end of 'switch' on SectorNumber ------------------------------------------------------ end 'switch' on SectorNumber
	} // end 'if' on  (SectorNumber == 1 || SectorNumber == 7)

	if (SectorNumber == 3 || SectorNumber == 5)
	{
	  switch (SectorNumber)
	  {
	case 3: { // +++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++ 'case 1'
		if (saAxisConfineData == "<- Goal[2][0]") // GAME's Z coordinant is below GOAL Z0 plane
		{
		for (int i{ 0 }; i < 3; ++i)
		{
			if (i == 1)
			{
			  DeltaXYZ[i] = 0;
			  continue; // the GAME icon is between the two GOAL Y axes so no change in GAME’s Y position is needed 
			}
			else {
				DeltaXYZ[i] = Goal[i][0] - Game[i];  // *(int(*)[3]DeltaXYZ  put this in watch window to see all of array values 
				DeltaXYZTotal += std::abs(DeltaXYZ[i]); // remember that arrays are passed to funcions only as pointers 
			}  // that's why C++'s class system is esar to use  
			for (int i{ 0 }; i < 3; ++i)
				DeltaPercent[i] = static_cast<double>(DeltaXYZ[i]) / DeltaXYZTotal;
		}
		}else if (saAxisConfineData == "[Goal[2][0] Goal[2][1]]")  // GAME's Z coordinate is between GOAL's Z0 to Z1 planes 
		{
		 	for (int i{ 0 }; i < 2; ++i)
				{
					if (i == 1)
						DeltaXYZ[i] = 0;
					else if(i==0)
						DeltaXYZ[i] = Goal[i][0] - Game[i];
					else if(i==2)
					  DeltaXYZ[i] = 0;
					DeltaXYZTotal += std::abs(DeltaXYZ[i]);  
				}
			for (int i{ 0 }; i < 3; ++i)
				DeltaPercent[i] = static_cast<double>(DeltaXYZ[i]) / DeltaXYZTotal;
		}else if(saAxisConfineData == "-> Goal[2][1]") // GAME's Z coordinate is above GOAL's Z1 plane   
		  {
		    for (int i{ 0 }; i < 3; ++i)
		    {
			if (i == 1)
			{
			  DeltaXYZ[i] = 0;
			  continue; // the GAME icon is between the two GOAL's Y axes so no change in GAME’s Y position is needed 
			}
			else if(i == 0){
				DeltaXYZ[i] = Goal[i][0] - Game[i];  // *(int(*)[3]DeltaXYZ  put this in watch window to see all of array values 
				DeltaXYZTotal += std::abs(DeltaXYZ[i]); // remember that arrays are passed to funcions only as pointers 
			}else if(i == 2)  // that's why C++'s class system is esar to use  
			{
			    DeltaXYZ[i] = Goal[i][1] - Game[i];  // *(int(*)[3]DeltaXYZ  put this in watch window to see all of array values 
				DeltaXYZTotal += std::abs(DeltaXYZ[i]); // remember that arrays are passed to funcions only as pointers 
			}
			for (int i{ 0 }; i < 3; ++i)
				DeltaPercent[i] = static_cast<double>(DeltaXYZ[i]) / DeltaXYZTotal;
		}
		  }
		break;  // *(<type>(*)[arrSize])arrName == *(int(*)[3][2])Goal
	   }// end 'case' 1 -------------------------------------------------------------------------- 'case' 1
	 
	case 5: { // +++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++ 'case 7'
		if (saAxisConfineData == "<- Goal[2][0]") // GAME's Z coordinant is below GOAL Z0 plane
		{
		for (int i{ 0 }; i < 3; ++i)
		{
			if (i == 1)
			{
			  DeltaXYZ[i] = 0;
			  continue; // the GAME icon is between the two GOAL's Y axes so no change in GAME’s Y position is needed 
			}
			else if(i==0){
				DeltaXYZ[i] = Goal[i][1] - Game[i];  // *(int(*)[3]DeltaXYZ  put this in watch window to see all of array values 
				DeltaXYZTotal += std::abs(DeltaXYZ[i]); // remember that arrays are passed to funcions only as pointers 
			}  // that's why C++'s class system is esar to use  
			else if(i==2){
			    DeltaXYZ[i] = Goal[i][0] - Game[i];  // *(int(*)[3]DeltaXYZ  put this in watch window to see all of array values 
				DeltaXYZTotal += std::abs(DeltaXYZ[i]); // remember that arrays are passed to funcions only as pointers 
			}
		}
			for (int i{ 0 }; i < 3; ++i)
				DeltaPercent[i] = static_cast<double>(DeltaXYZ[i]) / DeltaXYZTotal;
		
		}else if (saAxisConfineData == "[Goal[2][0] Goal[2][1]]")  // GAME's Z coordinate is between GOAL's Z0 to Z1 planes 
		{
		 	for (int i{ 0 }; i < 2; ++i)
				{
					if (i == 1)
						DeltaXYZ[i] = 0;
					else if(i==0)
						DeltaXYZ[i] = Goal[i][1] - Game[i];
					else if(i==2)
					  DeltaXYZ[i] = 0;
					DeltaXYZTotal += std::abs(DeltaXYZ[i]);  
				}
			for (int i{ 0 }; i < 3; ++i)
				DeltaPercent[i] = static_cast<double>(DeltaXYZ[i]) / DeltaXYZTotal;
		}else if(saAxisConfineData == "-> Goal[2][1]") // GAME's Z coordinate is above GOAL's Z1 plane   
		  {
		    for (int i{ 0 }; i < 3; ++i)
		    {
			if (i == 1)
			{
			  DeltaXYZ[i] = 0;
			  continue; // the GAME icon is between the two GOAL Y axes so no change in GAME’s Y position is needed 
			}
			else if(i == 0){
				DeltaXYZ[i] = Goal[i][1] - Game[i];  // *(int(*)[3]DeltaXYZ  put this in watch window to see all of array values 
				DeltaXYZTotal += std::abs(DeltaXYZ[i]); // remember that arrays are passed to funcions only as pointers 
			}
			else if(i == 2)  // that's why C++'s class system is esar to use  
			{
			    DeltaXYZ[i] = Goal[i][1] - Game[i];  // *(int(*)[3]DeltaXYZ  put this in watch window to see all of array values 
				DeltaXYZTotal += std::abs(DeltaXYZ[i]); // remember that arrays are passed to funcions only as pointers 
			} 
			for (int i{ 0 }; i < 3; ++i)
				DeltaPercent[i] = static_cast<double>(DeltaXYZ[i]) / DeltaXYZTotal;
		}
		  }
		break;
		}
       }// end of 'switch' on SectorNumber ------------------------------------------------------ end 'switch' on SectorNumber
	} // end 'if' on  (SectorNumber == 1 || SectorNumber == 7)

	if (SectorNumber == 4)  // GAME's ICON is directly over or under the GOAL so only Z axis movement is needed
	{
	    DeltaXYZ[0] = DeltaXYZ[1] = 0; // no X or Y axes movement is needed
	    
	    if (saAxisConfineData == "<- Goal[2][0]") // GAME's Z coordinant is below GOAL Z0 plane
		{
		 DeltaXYZ[2] = Goal[2][0] - Game[2];  // *(int(*)[3]DeltaXYZ  put this in watch window to see all of array values 
		 DeltaXYZTotal += std::abs(DeltaXYZ[2]); // remember that arrays are passed to funcions only as pointers 
		 
		// no 'if (saAxisConfineData == "[Goal[2][0] Goal[2][1]]")' in SectorNumber == 4 as this would have GAME in the GOAL
        }else if(saAxisConfineData == "-> Goal[2][1]") // GAME's Z coordinate is above GOAL's Z1 plane   
		  {
		 DeltaXYZ[2] = Goal[2][1] - Game[2];  // *(int(*)[3]DeltaXYZ  put this in watch window to see all of array values 
		 DeltaXYZTotal += std::abs(DeltaXYZ[2]); // remember that arrays are passed to funcions only as pointers 
		  }
		  
		  	for (int i{ 0 }; i < 3; ++i)
				DeltaPercent[i] = static_cast<double>(DeltaXYZ[i]) / DeltaXYZTotal;
		
	} // end 'if' on  (SectorNumber == 4)


}


// END calculateGamesMove calculateGamesMove ENDS -----------------------------------------------------------------------------------
// ----------------------------------------------------------------------------------------------------------------------------------




/***********************************************************************************************************************************
 * OLD THIS WORKS FOR SECTOR G0 ONLY USE TO BUILD A FUNCTION THAT WILL TAKE ARGUMENTS FOR ALL SECTORS ******************************
void calculateGamesMove(int SectorNumber,  std::string saAxisConfineData, int DeltaXYZ[], int Goal[][2], int Game[], int &DeltaXYZTotal, double DeltaPercent[])
{
	*****
		int Goal[3][2]{ {5,10}, {5, 10},{5, 10} };
		int Game[3]{ 0 , 0, 0 };
		int DeltaXYZ[3]{ -1,-1,-1};
		double DeltaPercent[3]{ -1, -1, -1};
		*****
*****
XYZ? == G0, G2, G6, G8
YZ?  == G1, G7
XZ?  == G3, G5
Z    == G4
*****
	if (SectorNumber == 0 || SectorNumber == 2 || SectorNumber == 6 || SectorNumber == 8)
	{
		if (saAxisConfineData == "<- Goal[2][0]" ) // below GOAL
		{
			for (int i{ 0 }; i < 3; ++i)
			{
				DeltaXYZ[i] = Goal[i][0] - Game[i];
					DeltaXYZTotal += DeltaXYZ[i];
			}
			for (int i{ 0 }; i < 3; ++i)
				DeltaPercent[i] = static_cast<double>(DeltaXYZ[i]) / DeltaXYZTotal;
		}
		else if (saAxisConfineData == "[Goal[2][0] Goal[2][1]]") // in same plane as GOAL
		{
			for (int i{ 0 }; i < 2; ++i)
			{
				DeltaXYZ[i] = Goal[i][0] - Game[i];
				DeltaXYZTotal += DeltaXYZ[i];
			}
			for (int i{ 0 }; i < 2; ++i)
				DeltaPercent[i] = static_cast<double>(DeltaXYZ[i]) / DeltaXYZTotal;

		}
		else if (saAxisConfineData == "-> Goal[2][1]") // above the GOAL
		{
			for (int i{ 0 }; i < 2; ++i)
					 {
				DeltaXYZ[i] = Goal[i][0] - Game[i];
				DeltaXYZTotal += DeltaXYZ[i];
					 }
						DeltaXYZ[2] = Goal[2][1] - Game[2];
				DeltaXYZTotal += std::abs(DeltaXYZ[2]);
						for (int i{ 0 }; i < 3; ++i)
					DeltaPercent[i] = static_cast<double>(DeltaXYZ[i]) / DeltaXYZTotal;
		}
	}
}
*****************************************************************************************************************************************
*****************************************************************************************************************************************/
/*****
XYZ? == G0, G2, G6, G8
YZ?  == G1, G7
XZ?  == G3, G5
Z    == G4
*****/
