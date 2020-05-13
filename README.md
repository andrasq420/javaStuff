import java.util.Random;


class InvalidDotNumberException extends Exception {

}

public class DominoTile {
    private int left;
    private int right;

    public DominoTile() {
        Random x = new Random();
        left = 1+x.nextInt(5);
        right = 1+x.nextInt(5);
    }

    public DominoTile(int l, int r) throws InvalidDotNumberException {
        left = l;
        right = r;
        if(left >6 || left <1 || right>6 || right < 1) {
            throw new InvalidDotNumberException();
        }
    }

    public void rotate() {
        int buffer;
        buffer = left;
        left = right;
        right = buffer;
    }

     public boolean canConnectFromLeft(DominoTile other) {
        if(other.left == right){
            return true;
        }
        else
            return false;
     }

    public static void main(String[] args) {
        try {
            DominoTile d11=new DominoTile(1,1);
            System.out.println("[OK] [1|1] domino tile created successfully.");
            try {
                DominoTile d12=new DominoTile(1,2);
                System.out.println("[OK] [1|2] domino tile created successfully.");
                try {
                    DominoTile d17=new DominoTile(1,7);
                    System.err.println("[ERROR] [1|7] domino tile created successfully.");
                } catch (InvalidDotNumberException e){
                    System.out.println("[OK] Domino [1|7] could not be created.");
                }
                if(d11.canConnectFromLeft(d12)) System.out.println("[OK] [1|1] can connect to [1|2]");
                else System.err.println("[ERROR] [1|1] can not connect to [1|2]");
                if(!d12.canConnectFromLeft(d11)) System.out.println("[OK] [1|2] cannot connect to [1|1]");
                else System.err.println("[ERROR] [1|2] can connect to [1|1]");
                d12.rotate();
                if(!d11.canConnectFromLeft(d12)) System.out.println("[OK] [1|1] cannot connect to [2|1]");
                else System.err.println("[ERROR] [1|1] can not connect to [2|1]");
                if(d12.canConnectFromLeft(d11)) System.out.println("[OK] [2|1] can connect to [1|1]");
                else System.err.println("[ERROR] [2|1] cannot connect to [1|1]");
            } catch (InvalidDotNumberException e){
                System.err.println("[ERROR] Domino [1|2] could not be created.");
            }
        } catch (InvalidDotNumberException e){
            System.err.println("[ERROR] Domino [1|1] could not be created.");
        }
    }
}





// 2. rész

import java.util.ArrayList;
import java.util.Scanner;

class InvalidMoveException extends Exception {

}
public class DominoGame {

    DominoTile starter;
    DominoTile d1;
    DominoTile d2;
    DominoTile d3;
    ArrayList<DominoTile> dominos = new ArrayList<DominoTile>();

    public DominoGame() {
        starter = new DominoTile();
        d1 = new DominoTile();
        d2 = new DominoTile();
        d3 = new DominoTile();
        dominos.add(starter);

    }



    public DominoTile getTileOption(int number) {
        if(number == 1 ) {
            return d1;
        }
        else {
            if(number == 2) {
                return d2;
            }
            else
                return d3;
        }

    }

    public void continueWithOption(int number) throws InvalidMoveException {
        DominoTile last = dominos .get(dominos.size() - 1);
        boolean canContinue = false;
        canContinue = last.canConnectFromLeft(getTileOption(number));
        if(canContinue) {
            dominos.add(getTileOption(number));
        }
        else
            throw new InvalidMoveException();
    }

    public boolean isGameOver() {
        DominoTile last = dominos.get(dominos.size()-1);
        System.out.println(last);
        if (last.canConnectFromLeft(getTileOption(1)) == false &&
                last.canConnectFromLeft(getTileOption(2)) == false &&
                last.canConnectFromLeft(getTileOption(3)) == false) {   // ha egyik se tud csatlakozni akkor game over
            return true;
        }
        else
            return false;
    }




    public static void main(String[] args) {
        DominoGame game = new DominoGame();
        Scanner sc=new Scanner(System.in);
        while(!game.isGameOver()) {
            System.out.println(game);
            System.out.print("Which domino should I use? (1-3) ");
            int domino = sc.nextInt();
            try{
                game.continueWithOption(domino);
            } catch (InvalidMoveException e) {
                System.out.println("Domino "+game.getTileOption(domino)+" can not be used to continue.");
            }
        }
        System.err.println(game);
        System.out.println("Game over");
    }
}







