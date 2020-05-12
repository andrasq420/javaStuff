import java.util.Arrays;

class EmptyTowerException extends Exception{

}

class InvalidMoveException extends Exception{

}

public class HanoiTower {
    private int magassag = 3;
    private int[] Torony = new int[magassag];


    public HanoiTower() {
        for (int i = 0; i < Torony.length; i++) {
            Torony[i] = 0;
        }

    }

    public HanoiTower(int h) {
        for (int i = 0; i < h; i++) {
            magassag = h;
            Torony[i] = h-i;
        }
    }

    public int pop() throws EmptyTowerException {
        int meret=0;
        for (int i = magassag-1; i < -1; i--) {
            if (Torony[i] > 0) {
                meret = Torony[i];
                Torony[i] = 0;
                break;
            }
        }
        if(meret == 0){
            throw new EmptyTowerException();
        }
        else {
            return meret;
        }
    }

    public void put(int size) throws InvalidMoveException {
        for (int i = 0; i < Torony.length ; i++) {
            if(Torony[i] ==0) {
                Torony[i] = size;
                break;
            }
            else
                if(Torony[i] < size)
                    throw new InvalidMoveException();
        }
    }


    public static void main(String[] args) {
        HanoiTower test= new HanoiTower();
        try{
            test.pop();
            System.err.println("[ERROR] pop succesful from empty tower");
        } catch (EmptyTowerException e){
            System.err.println("[OK] pop unsuccesful from empty tower");
        }
        try{
            test.put(6);
            System.err.println("[OK] put 6 succesful on empty tower");
        } catch (InvalidMoveException e){
            System.err.println("[ERROR] put 6 succesful on empty tower");
        }
        try{
            test.put(4);
            System.err.println("[OK] put 4 succesful on tower [6]");
        } catch (InvalidMoveException e){
            System.err.println("[ERROR] put 4 succesful on tower [6]");
        }
        try{
            test.put(1);
            System.err.println("[OK] put 1 succesful on tower [6 4]");
        } catch (InvalidMoveException e){
            System.err.println("[ERROR] put 1 succesful on tower [6 4]");
        }
        try{
            test.put(2);
            System.err.println("[ERROR] put 2 succesful on tower [6 4 1]");
        } catch (InvalidMoveException e){
            System.err.println("[OK] put 1 succesful on tower [6 4 1]");
        }
        System.out.println(Arrays.toString(test.Torony));
    }

}
