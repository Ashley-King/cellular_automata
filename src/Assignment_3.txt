/*
 * Course:   CS 1B Fall 2016
 * Student: Ashley King
 * Program: Cellular Automata
 */
import java.util.Scanner;

public class Foothill
{

   public static void main(String[] args)
   {
      int rule;
      String strUserIn;

      Scanner inputStream = new Scanner(System.in);
      Automaton aut;

      //get rule from user
      do {
         System.out.print("Enter Rule (0-255): ");
         //get user answer in form of a string
         strUserIn = inputStream.nextLine();
         //convert it to a number
         rule = Integer.parseInt(strUserIn);
      }while(rule < 0 || rule > 255);
      inputStream.close();
      //create new automaton
      aut = new Automaton(rule);
      //create new generations
      System.out.println("-- Begin Propagation --");
      for(int i = 0; i < 100; i++)
      {
         System.out.println(aut.toStringCurrentGen());
         aut.propagateNewGeneration();
      }
      System.out.println("-- End Propagation --");
   }//end main
}//end Foothill

class Automaton
{
   //class constants
   public final static int MAX_DISPLAY_WIDTH = 121;
   //typical displayWidth
   public final static int TYPICAL_WIDTH = 79;

   //private members
   private boolean rules[];
   private StringBuffer thisGen;
   private String extremeBit;
   private int displayWidth;

   //public constructors
   public Automaton(int new_rule)
   {
      //sanitize rule & convert to internal representation
      if(!setRule(new_rule))
      {
         setRule(0);
      }
      this.extremeBit = " ";
      this.thisGen = new StringBuffer("*");
   }

   public void resetFirstGen()
   {
      this.thisGen = new StringBuffer("*");
   }

   public boolean setRule(int new_rule)
   {
      //create mask to use with bitwise operators
      int mask = 1;
      if(new_rule >= 0 && new_rule <= 255)
      {
         rules = new boolean[8];
         for(int i = 0; i < rules.length; i++)
         {
            if((new_rule & mask) != 0)
            {
               rules[i] = true;
            }
            else
            {
               rules[i] = false;
            }
            mask = mask << 1;
         }
         return true;
      }
      else
      {
         return false;
      }   
   }

   public boolean setDisplayWidth(int width)
   {
      if(width <= MAX_DISPLAY_WIDTH && (width % 2) != 0)
      {
         this.displayWidth = width;
         return true;
      }
      else
      {
         return false;
      }
   }

   public StringBuffer toStringCurrentGen()
   {
      setDisplayWidth(TYPICAL_WIDTH);
      StringBuffer returnString = new StringBuffer(displayWidth);
      if(thisGen.length() < displayWidth)
      {

         //amount of padding on either side of thisGen
         int numPad = displayWidth - thisGen.length();
         numPad = numPad / 2;
         for(int i = 0; i < numPad; i++)
         {
            returnString.append(extremeBit);
         }
         returnString.append(thisGen);
         for(int i = 0; i < numPad; i++)
         {
            returnString.append(extremeBit);
         }
      }
      else if(thisGen.length() == displayWidth)
      {
         returnString = thisGen;
      }
      else
      {
         int excess = thisGen.length() - displayWidth;
         //get number of characters in excess on either side of middle string
         excess = excess / 2;
         //set indexes for substring
         int beginning = excess + 1;
         int end = thisGen.length() - excess + 1;
         //set substring
         returnString.append(thisGen.substring(beginning, end));
      }
      return returnString;
   }

   public void propagateNewGeneration()
   {
      StringBuffer nextGen = new StringBuffer("");
      //put two extremeBits together
      StringBuffer tempBits = new StringBuffer(extremeBit + extremeBit);
      //append two extremeBits to either side of thisGen 
      thisGen = thisGen.insert(0, tempBits);
      thisGen.append(tempBits);
      //apply rule and put new generation in nextGen
      StringBuffer tempStr;
      //check to see if we're at the end of thisGen
      for(int i = 0; i < thisGen.length() - 1; i++)
      {
         //get substring
         tempStr = new StringBuffer("");
         if(i == thisGen.length() - 2)
         {
            break;
         }
         else
         {
            tempStr.append(thisGen.substring(i, i+3));
         } 
         //test for & get value of substring
         for(int k = 2; k >= 0; k--)
         {
            if(tempStr.charAt(k) ==' ')
            {
               tempStr.replace(k, k+1, "0");
            }
            else
            {
               tempStr.replace(k, k+1, "1");
            }
         }
         //get binary value of tempStr
         int tempStrValue = Integer.parseInt(String.valueOf(tempStr), 2);
         //save next generation status 
         boolean nextLife = rules[tempStrValue];
         //if true, append *; if false append ""
         if(nextLife == true)
         {
            nextGen.append('*');
         }
         else
         {
            nextGen.append(' ');
         }
      }//end for to apply rule
      thisGen = nextGen;
      //get triplet of extremeBits
      tempBits.append(extremeBit);
      //test for & get value of extremeBit triplet
      for(int k = 2; k >= 0; k--)
      {
         if(tempBits.charAt(k) ==' ')
         {
            tempBits.replace(k, k+1, "0");
         }
         else
         {
            tempBits.replace(k, k+1, "1");
         }
      }
      //get binary value of tempStr
      int extremeBitValue = Integer.parseInt(String.valueOf(tempBits), 2);
      //save next generation status 
      boolean nextBit = rules[extremeBitValue];
      //if true, extremeBit is *; if false extremeBit is " "
      if(nextBit == true)
      {
         extremeBit = "*";
      }
      else
      {
         extremeBit = " ";
      }

   }//end propogateNewGeneration


}//end class Automaton

/***************** OUPUT (4, 126, 130, 130) ***********************************
Enter Rule (0-255): 4
-- Begin Propagation --
                                       *                                       
                                       *                                       
                                       *                                       
                                       *                                       
                                       *                                       
                                       *                                       
                                       *                                       
                                       *                                       
                                       *                                       
                                       *                                       
                                       *                                       
                                       *                                       
                                       *                                       
                                       *                                       
                                       *                                       
                                       *                                       
                                       *                                       
                                       *                                       
                                       *                                       
                                       *                                       
                                       *                                       
                                       *                                       
                                       *                                       
                                       *                                       
                                       *                                       
                                       *                                       
                                       *                                       
                                       *                                       
                                       *                                       
                                       *                                       
                                       *                                       
                                       *                                       
                                       *                                       
                                       *                                       
                                       *                                       
                                       *                                       
                                       *                                       
                                       *                                       
                                       *                                       
                                       *                                       
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
                                      *                                        
-- End Propagation --
Enter Rule (0-255): 126
-- Begin Propagation --
                                       *                                       
                                      ***                                      
                                     ** **                                     
                                    *******                                    
                                   **     **                                   
                                  ****   ****                                  
                                 **  ** **  **                                 
                                ***************                                
                               **             **                               
                              ****           ****                              
                             **  **         **  **                             
                            ********       ********                            
                           **      **     **      **                           
                          ****    ****   ****    ****                          
                         **  **  **  ** **  **  **  **                         
                        *******************************                        
                       **                             **                       
                      ****                           ****                      
                     **  **                         **  **                     
                    ********                       ********                    
                   **      **                     **      **                   
                  ****    ****                   ****    ****                  
                 **  **  **  **                 **  **  **  **                 
                ****************               ****************                
               **              **             **              **               
              ****            ****           ****            ****              
             **  **          **  **         **  **          **  **             
            ********        ********       ********        ********            
           **      **      **      **     **      **      **      **           
          ****    ****    ****    ****   ****    ****    ****    ****          
         **  **  **  **  **  **  **  ** **  **  **  **  **  **  **  **         
        ***************************************************************        
       **                                                             **       
      ****                                                           ****      
     **  **                                                         **  **     
    ********                                                       ********    
   **      **                                                     **      **   
  ****    ****                                                   ****    ****  
 **  **  **  **                                                 **  **  **  ** 
****************                                               ****************
              **                                             **              **
*            ****                                           ****            ***
**          **  **                                         **  **          **  
***        ********                                       ********        *****
  **      **      **                                     **      **      **    
 ****    ****    ****                                   ****    ****    ****   
**  **  **  **  **  **                                 **  **  **  **  **  **  
***********************                               *************************
                      **                             **                        
                     ****                           ****                       
                    **  **                         **  **                      
                   ********                       ********                     
                  **      **                     **      **                    
                 ****    ****                   ****    ****                   
                **  **  **  **                 **  **  **  **                  
               ****************               ****************                *
              **              **             **              **              **
*            ****            ****           ****            ****            ***
**          **  **          **  **         **  **          **  **          **  
***        ********        ********       ********        ********        *****
  **      **      **      **      **     **      **      **      **      **    
 ****    ****    ****    ****    ****   ****    ****    ****    ****    ****   
**  **  **  **  **  **  **  **  **  ** **  **  **  **  **  **  **  **  **  **  
*******************************************************************************
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                              *
                                                                             **
*                                                                           ***
**                                                                         **  
***                                                                       *****
  **                                                                     **    
 ****                                                                   ****   
**  **                                                                 **  **  
*******                                                               *********
      **                                                             **        
     ****                                                           ****       
    **  **                                                         **  **      
   ********                                                       ********     
-- End Propagation --

Enter Rule (0-255): 130
-- Begin Propagation --
                                       *                                       
                                      *                                        
                                     *                                         
                                    *                                          
                                   *                                           
                                  *                                            
                                 *                                             
                                *                                              
                               *                                               
                              *                                                
                             *                                                 
                            *                                                  
                           *                                                   
                          *                                                    
                         *                                                     
                        *                                                      
                       *                                                       
                      *                                                        
                     *                                                         
                    *                                                          
                   *                                                           
                  *                                                            
                 *                                                             
                *                                                              
               *                                                               
              *                                                                
             *                                                                 
            *                                                                  
           *                                                                   
          *                                                                    
         *                                                                     
        *                                                                      
       *                                                                       
      *                                                                        
     *                                                                         
    *                                                                          
   *                                                                           
  *                                                                            
 *                                                                             
*                                                                              
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
                                                                               
-- End Propagation --
Enter Rule (0-255): 131
-- Begin Propagation --
                                       *                                       
***************************************  **************************************
**************************************  * *************************************
*************************************  *   ************************************
************************************  *  ** ***********************************
***********************************  *  *    **********************************
**********************************  *  *  *** *********************************
*********************************  *  *  * *   ********************************
********************************  *  *  *    ** *******************************
*******************************  *  *  *  ***    ******************************
******************************  *  *  *  * *  *** *****************************
*****************************  *  *  *  *    * *   ****************************
****************************  *  *  *  *  ***    ** ***************************
***************************  *  *  *  *  * *  ***    **************************
**************************  *  *  *  *  *    * *  *** *************************
*************************  *  *  *  *  *  ***    * *   ************************
************************  *  *  *  *  *  * *  ***    ** ***********************
***********************  *  *  *  *  *  *    * *  ***    **********************
**********************  *  *  *  *  *  *  ***    * *  *** *********************
*********************  *  *  *  *  *  *  * *  ***    * *   ********************
********************  *  *  *  *  *  *  *    * *  ***    ** *******************
*******************  *  *  *  *  *  *  *  ***    * *  ***    ******************
******************  *  *  *  *  *  *  *  * *  ***    * *  *** *****************
*****************  *  *  *  *  *  *  *  *    * *  ***    * *   ****************
****************  *  *  *  *  *  *  *  *  ***    * *  ***    ** ***************
***************  *  *  *  *  *  *  *  *  * *  ***    * *  ***    **************
**************  *  *  *  *  *  *  *  *  *    * *  ***    * *  *** *************
*************  *  *  *  *  *  *  *  *  *  ***    * *  ***    * *   ************
************  *  *  *  *  *  *  *  *  *  * *  ***    * *  ***    ** ***********
***********  *  *  *  *  *  *  *  *  *  *    * *  ***    * *  ***    **********
**********  *  *  *  *  *  *  *  *  *  *  ***    * *  ***    * *  *** *********
*********  *  *  *  *  *  *  *  *  *  *  * *  ***    * *  ***    * *   ********
********  *  *  *  *  *  *  *  *  *  *  *    * *  ***    * *  ***    ** *******
*******  *  *  *  *  *  *  *  *  *  *  *  ***    * *  ***    * *  ***    ******
******  *  *  *  *  *  *  *  *  *  *  *  * *  ***    * *  ***    * *  *** *****
*****  *  *  *  *  *  *  *  *  *  *  *  *    * *  ***    * *  ***    * *   ****
****  *  *  *  *  *  *  *  *  *  *  *  *  ***    * *  ***    * *  ***    ** ***
***  *  *  *  *  *  *  *  *  *  *  *  *  * *  ***    * *  ***    * *  ***    **
**  *  *  *  *  *  *  *  *  *  *  *  *  *    * *  ***    * *  ***    * *  *** *
*  *  *  *  *  *  *  *  *  *  *  *  *  *  ***    * *  ***    * *  ***    * *   
 *  *  *  *  *  *  *  *  *  *  *  *  *  * *  ***    * *  ***    * *  ***    ** 
*  *  *  *  *  *  *  *  *  *  *  *  *  *    * *  ***    * *  ***    * *  ***   
  *  *  *  *  *  *  *  *  *  *  *  *  *  ***    * *  ***    * *  ***    * *  **
 *  *  *  *  *  *  *  *  *  *  *  *  *  * *  ***    * *  ***    * *  ***    * *
*  *  *  *  *  *  *  *  *  *  *  *  *  *    * *  ***    * *  ***    * *  ***   
  *  *  *  *  *  *  *  *  *  *  *  *  *  ***    * *  ***    * *  ***    * *  **
 *  *  *  *  *  *  *  *  *  *  *  *  *  * *  ***    * *  ***    * *  ***    * *
*  *  *  *  *  *  *  *  *  *  *  *  *  *    * *  ***    * *  ***    * *  ***   
  *  *  *  *  *  *  *  *  *  *  *  *  *  ***    * *  ***    * *  ***    * *  **
 *  *  *  *  *  *  *  *  *  *  *  *  *  * *  ***    * *  ***    * *  ***    * *
*  *  *  *  *  *  *  *  *  *  *  *  *  *    * *  ***    * *  ***    * *  ***   
  *  *  *  *  *  *  *  *  *  *  *  *  *  ***    * *  ***    * *  ***    * *  **
 *  *  *  *  *  *  *  *  *  *  *  *  *  * *  ***    * *  ***    * *  ***    * *
*  *  *  *  *  *  *  *  *  *  *  *  *  *    * *  ***    * *  ***    * *  ***   
  *  *  *  *  *  *  *  *  *  *  *  *  *  ***    * *  ***    * *  ***    * *  **
 *  *  *  *  *  *  *  *  *  *  *  *  *  * *  ***    * *  ***    * *  ***    * *
*  *  *  *  *  *  *  *  *  *  *  *  *  *    * *  ***    * *  ***    * *  ***   
  *  *  *  *  *  *  *  *  *  *  *  *  *  ***    * *  ***    * *  ***    * *  **
 *  *  *  *  *  *  *  *  *  *  *  *  *  * *  ***    * *  ***    * *  ***    * *
*  *  *  *  *  *  *  *  *  *  *  *  *  *    * *  ***    * *  ***    * *  ***   
  *  *  *  *  *  *  *  *  *  *  *  *  *  ***    * *  ***    * *  ***    * *  **
 *  *  *  *  *  *  *  *  *  *  *  *  *  * *  ***    * *  ***    * *  ***    * *
*  *  *  *  *  *  *  *  *  *  *  *  *  *    * *  ***    * *  ***    * *  ***   
  *  *  *  *  *  *  *  *  *  *  *  *  *  ***    * *  ***    * *  ***    * *  **
 *  *  *  *  *  *  *  *  *  *  *  *  *  * *  ***    * *  ***    * *  ***    * *
*  *  *  *  *  *  *  *  *  *  *  *  *  *    * *  ***    * *  ***    * *  ***   
  *  *  *  *  *  *  *  *  *  *  *  *  *  ***    * *  ***    * *  ***    * *  **
 *  *  *  *  *  *  *  *  *  *  *  *  *  * *  ***    * *  ***    * *  ***    * *
*  *  *  *  *  *  *  *  *  *  *  *  *  *    * *  ***    * *  ***    * *  ***   
  *  *  *  *  *  *  *  *  *  *  *  *  *  ***    * *  ***    * *  ***    * *  **
 *  *  *  *  *  *  *  *  *  *  *  *  *  * *  ***    * *  ***    * *  ***    * *
*  *  *  *  *  *  *  *  *  *  *  *  *  *    * *  ***    * *  ***    * *  ***   
  *  *  *  *  *  *  *  *  *  *  *  *  *  ***    * *  ***    * *  ***    * *  **
 *  *  *  *  *  *  *  *  *  *  *  *  *  * *  ***    * *  ***    * *  ***    * *
*  *  *  *  *  *  *  *  *  *  *  *  *  *    * *  ***    * *  ***    * *  ***   
  *  *  *  *  *  *  *  *  *  *  *  *  *  ***    * *  ***    * *  ***    * *  **
 *  *  *  *  *  *  *  *  *  *  *  *  *  * *  ***    * *  ***    * *  ***    * *
*  *  *  *  *  *  *  *  *  *  *  *  *  *    * *  ***    * *  ***    * *  ***   
  *  *  *  *  *  *  *  *  *  *  *  *  *  ***    * *  ***    * *  ***    * *  **
 *  *  *  *  *  *  *  *  *  *  *  *  *  * *  ***    * *  ***    * *  ***    * *
*  *  *  *  *  *  *  *  *  *  *  *  *  *    * *  ***    * *  ***    * *  ***   
  *  *  *  *  *  *  *  *  *  *  *  *  *  ***    * *  ***    * *  ***    * *  **
 *  *  *  *  *  *  *  *  *  *  *  *  *  * *  ***    * *  ***    * *  ***    * *
*  *  *  *  *  *  *  *  *  *  *  *  *  *    * *  ***    * *  ***    * *  ***   
  *  *  *  *  *  *  *  *  *  *  *  *  *  ***    * *  ***    * *  ***    * *  **
 *  *  *  *  *  *  *  *  *  *  *  *  *  * *  ***    * *  ***    * *  ***    * *
*  *  *  *  *  *  *  *  *  *  *  *  *  *    * *  ***    * *  ***    * *  ***   
  *  *  *  *  *  *  *  *  *  *  *  *  *  ***    * *  ***    * *  ***    * *  **
 *  *  *  *  *  *  *  *  *  *  *  *  *  * *  ***    * *  ***    * *  ***    * *
*  *  *  *  *  *  *  *  *  *  *  *  *  *    * *  ***    * *  ***    * *  ***   
  *  *  *  *  *  *  *  *  *  *  *  *  *  ***    * *  ***    * *  ***    * *  **
 *  *  *  *  *  *  *  *  *  *  *  *  *  * *  ***    * *  ***    * *  ***    * *
*  *  *  *  *  *  *  *  *  *  *  *  *  *    * *  ***    * *  ***    * *  ***   
  *  *  *  *  *  *  *  *  *  *  *  *  *  ***    * *  ***    * *  ***    * *  **
 *  *  *  *  *  *  *  *  *  *  *  *  *  * *  ***    * *  ***    * *  ***    * *
*  *  *  *  *  *  *  *  *  *  *  *  *  *    * *  ***    * *  ***    * *  ***   
  *  *  *  *  *  *  *  *  *  *  *  *  *  ***    * *  ***    * *  ***    * *  **
 *  *  *  *  *  *  *  *  *  *  *  *  *  * *  ***    * *  ***    * *  ***    * *
*  *  *  *  *  *  *  *  *  *  *  *  *  *    * *  ***    * *  ***    * *  ***   
  *  *  *  *  *  *  *  *  *  *  *  *  *  ***    * *  ***    * *  ***    * *  **
-- End Propagation --



*******************************************************************************/