# Debugging


1. Why does the board only add horizontal but not vertical battleships?
- Det er fordi, at metoden som ses nedenstående kun kaldte til horizontal, da den kunne kaldte på 0. Dette kunne man se, når vi evaluerede på det, at vertialt aldrig blev kaldt. 

private Point calculateEndPoint(Point startPoint) {
    return switch (getRandomInteger(1)) {
        case 0 -> getHorizontalEndPoint(startPoint);
        case 1 -> getVerticalEndPoint(startPoint);
        default -> throw new IllegalStateException("Unexpected value.");
    };

Derfor tilføjede jeg + 1 til denne metode: 
private static int getRandomInteger(int maxValue) {
    return new Random().nextInt(maxValue + 1);
}

De teknikker jeg anvendte her: 
- Breakpoint ved BattleshipGame.setupBoad();
- Der bruges Step over, Step in og Step out. 
- Evaluate, tilføjes + 1, for at se om det det spytter tal ud mellem 0 og 1. 

Ved evaluate kun man se, at det så ændre til både 0 og 1. 
    
2. Why do we never get message *Ship sunk!* when log output shows 3 direct hits?
- Problemet her er, at isSunk() metoden altid var 0. Og det vil sige, at det blev aldrig registret når et skib blev ramt. Derfor lavede jeg en conditional breakpoint i metoden for at pause eller stoppe spillet, når et skib bliver ramt 3 gange. Jeg tilføjede til conditional breakpoint hitCount == 3. 

 public boolean isSunk() {
        return hitCount >= BATTLESHIP_LENGTH;
    }



De teknikker jeg anvendte her: 
- En ny breakpoint
- Tilføjer hitcoint == 3 til metoden vha. conditional breakpoint. 
- Bruger resume program, til at køre progreammet for at se om det virker. 

3. Why do we sometimes see `java.lang.ArrayIndexOutOfBoundsException`?
- Jeg kørte debugger adskillige gange indtil så  exceptionen i addBattleship() metoden. Her kunne jeg se, at x = 9 & y = 10. 10 er over indeksen, da indeksen er 0-9. Derfor meldes fejlen også, at der bliver brugt en indeks, der ligger uden for arraysets grænser. 
Her skulle der en do-while løkke til at løse problemet og en hjælpemetode isWithinGrid(). Det hjalp med at startpunktet og slutpunktet altid lå inden for grænserne. Spillet meldte efterfølgende ingen fejl. 


De teknikker jeg anvendte her: 
- Tilføjer exception til breakpoint under view breakpoint (ctrl + shift + F8)
- Tilslut brugte jeg "Stop" knappen, for afslutte debugging. 
