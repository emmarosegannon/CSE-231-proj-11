# CSE-231-proj-11
    ###########################################################
    #  Computer Project # 11
    #    
    #    Have players take turns placing pieces
    #    Once player reaches win count in a constant row or diag
    #    Print winner and end game
    ###########################################################

class GoPiece(object):
    ''' Black and white pieces used in the game'''
    
    def __init__(self,color= 'black'):
        ''' This method creates a gomoku piece. Color is assigned'''
        
        color = color.strip().lower() ## will this be problemsome
        
        if color != 'black' and color != 'white': ## error checking or and?
            raise MyError('Wrong color.')
            
        else:
            
            self.__color = 'black' ## assign as precedent
        
            if color == 'white': ## unless user specifies white
                self.__color = 'white'
        
    def __str__(self):
        ''' Displays black or white piece as symbols'''
        
        if self.__color == 'black': ## if black, return black symbol
            
            return ' ● '
        
        else: ## else return white
            return ' ○ '
        
    def __repr__(self): ### DELETE THIS !!!!!!
        """ """
        return self.__str__()
    
    def get_color(self):
        ''' returns color of piece as string 'black' or 'white' '''
        if self.__color == 'black': ## if black piece, return string black
            return 'black'
        
        else: ## else, return white
            return 'white'
            
        
        
class MyError(Exception):
    '''when error is raised return it with this message'''
    def __init__(self,value):
        self.__value = value
    def __str__(self):
        return self.__value
    

class Gomoku(object):
    ''' sets board up, with displaying and for printing'''
    
    def __init__(self,board_size = 15 ,win_count = 5,current_player = 'black'):
        ''' creates new board'''
        
    ## board size
        if isinstance(board_size, float) == True: ## error checking
            raise ValueError
            
        else:
            board_size = int(board_size)
            self.__board_size = board_size ## set as default

    ## win count
    
        if isinstance(board_size, float) == True: ## error checking
            raise ValueError
            
        else:
            win_count = int(win_count)
            self.__win_count = win_count ## set as default     
        
    ## current_player
        if current_player != 'black' and current_player != 'white':
            raise MyError('Wrong color.')
            
        else:
            self.__current_player = current_player
            
        self.__go_board = [ [ ' - ' for j in range(self.__board_size)] \
                             for i in range(self.__board_size)]
         
            
    def assign_piece(self,piece,row,col):
        ''' places go piece at specified point'''
 
        try:
            if str(self.__go_board[row-1][col-1]) ==  ' ● ' or \
            str(self.__go_board[row-1][col-1]) == ' ○ ': ## raise error
                
                raise ValueError('Position is occupied.')
                
            if row in range(self.__board_size +1) and col in range\
            (self.__board_size +1): ## if it's within board size
                if self.__go_board[row-1][col-1] == ' - ':
                    self.__go_board[row-1][col-1] = piece
                    
        except ValueError:
            raise MyError('Position is occupied.')          
                    
        except:
            
            raise MyError('Invalid position.') ## not within board
       
            
    def get_current_player(self):
        ''' returns the current player as string 'black' or 'white' '''
        player = self.__current_player
        return player
    
    def switch_current_player(self):
        ''' returns opposite player'''
        
        current_player = self.__current_player #get current player
        if current_player == 'white':
            self.__current_player = 'black' ## switch
        else:
            self.__current_player = 'white' ## switch
            
        return str(self.__current_player)
    
    def __str__(self):
        ''' returns a string version'''
        s = '\n'
        for i,row in enumerate(self.__go_board):
            s += "{:>3d}|".format(i+1)
            for item in row:
                s += str(item)
            s += "\n"
        line = "___"*self.__board_size
        s += "    " + line + "\n"
        s += "    "
        for i in range(1,self.__board_size+1):
            s += "{:>3d}".format(i)
        s += "\n"
        s += 'Current player: ' + ('●' if self.__current_player == 'black' \
                                   else '○')
        return s
        
    def current_player_is_winner(self):
        '''Check to see is player is winner by checking rows horizontally,
        vertically, and diagonally. Return True or false'''
        ## declare objects
        
        player = ' ● ' if self.__current_player == 'black' else ' ○ '
        
        new_board = []
        winning_seq = '' #emp string
        
        for i in range(self.__win_count): ## create a str of the win sequence
            winning_seq +=  player 
        
        
        ## horizontal
        for point_list in self.__go_board:
            emp_list = []
            for point in point_list:
                if point != ' - ':
                    new_item = str(point)
                    emp_list.append(new_item)
                else:
                    emp_list.append(point)
                
            new_board.append(emp_list) #creates new board of str of all points
            
        
        ## vertical
        vert_list = []
        for row in range(self.__board_size):
            mini_list = []
            
            for colm in range(self.__board_size):
                mini_list.append(new_board[colm][row]) ## append in opp order
                
            vert_list.append(mini_list) ## then append so list of lists
            
            
        for i in range(self.__board_size): ## check if winning seq in vert list
            
            line_str = ''.join(vert_list[i])
            
            if winning_seq in line_str:
              
                return True
                
        
            
        for i in range(self.__board_size): ## check if winning sequence for
            line_str = ''.join(new_board[i]) ## horizontal is in list
            
            if winning_seq in line_str:
                
                return True
            
          ##diag winner        
            
        new_range = self.__board_size - self.__win_count + 1
        
        
        for row in range(0,new_range): ##use new range
            for col in range(self.__win_count - 1, self.__board_size):
                di_count = 0
                for win in range(0,self.__win_count):
                    if self.__go_board[row+win][col-win]: ## if point is
                        di_count += 1 ## assending order, plus 1
                        if self.__go_board[row+win][col-win] == ' - ':
                            di_count = 0 ## reset if there's a hole
                if di_count == self.__win_count: ## if it matches count, true
                  
                    return True
            
  
        
       ## diag winner
        for new_row in range(new_range): ## use new range
           
            for new_column in range(new_range):
               diag_count = 0 #empty count
               
               for win in range(self.__win_count):
                   
                   if str(self.__go_board[new_row+win][new_column+win]) == \
                   player: ## if it's decending in order and right order:
                       diag_count +=1   
               if diag_count == self.__win_count:
                   
                   return True #return true if it hits the win count
               
        return False ## else return false.
        
            
       
            
        
        
def main():
    ''' Have players take turns inputting where to put the Gopieces and
    once one player hits winner count, end game and declare winner'''
## objects
    board = Gomoku()
    print(board)
    piece = GoPiece()
    play = input("Input a row then column separated by a comma (q to quit): ")
    while play.lower() != 'q': ## if it doesn't equal quit
        play_list = play.strip().split(',') ## split these two
        try: 
            if len(play_list) != 2:
                raise MyError('Incorrect input.')    
                
            else:
            
                row = play_list[0].strip()
                col = play_list[1].strip()
                if row.isalpha() or col.isalpha():
                    raise MyError('Incorrect input.')
                else:
                    row = int(row)
                    col = int(col)
                    
                    #board.assign_piece(piece,row,col) 

            board.assign_piece(piece,row,col)

        except MyError as error_message: ## if error is raised
            print("{:s}\nTry again.".format(str(error_message)))
            print(board)
            
            play = input\
            ("Input a row then column separated by a comma (q to quit): ")
            continue
            
        #winner
        if board.current_player_is_winner(): ## if it passes function
            print(board)
            print("{} Wins!".format(board.get_current_player()))
            break#check
                                       
        else:
            board.switch_current_player() ## if no winner, continue playing
            piece = GoPiece(board.get_current_player())
            
        print(board)
        play = input\
        ("Input a row then column separated by a comma (q to quit): ")





if __name__ == '__main__':
    main()
