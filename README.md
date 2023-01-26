import java.io.File;
import java.io.FileNotFoundException;
import java.util.ArrayList;
import java.util.List;
import java.util.Random;
import java.util.Scanner;

public class Hangman {
	
	public static void main(String[] args) {
        try {
            Scanner scanner = new Scanner(new File("Hangmanadas"));
            Scanner keyboard = new Scanner(System.in);
            List<String> words = new ArrayList<String>();
            while (scanner.hasNext()) {
                words.add(scanner.nextLine());
            }
            int len = 5;
            String randomWord = getRandomWord(words, len);
            System.out.println(randomWord);
            List<Character> playerGuesses = new ArrayList<>();
            while(!printWordState(randomWord, playerGuesses)) {
                getPlayerGuess(keyboard, randomWord, playerGuesses);
            }
            System.out.println("Congratulations, you have guessed the word correctly!");
            displayHangMan(6);
        } catch (FileNotFoundException e) {
            System.out.println("File not found. Please check the file path.");
        }
	}
	
	static void displayHangMan(int incorrectGuesses) {
		System.out.println();

		switch (incorrectGuesses) {
		case 0:
			break;
		case 1:
			System.out.print("    _______\n");
			break;
		case 2:
			System.out.print("    _______\n");
			for (int i = 0; i < 6; i++)
				System.out.println("|        ||");
			break;
		case 3:
			System.out.println("    _______");
			for (int i = 0; i < 6; i++)
				System.out.println("|        ||");
			System.out.println("|________||");
			break;
		case 4:
			System.out.println("    _______");
			System.out.println("|   |    ||");
			System.out.println("|  \\O/   ||");

			for (int i = 0; i < 4; i++)
				System.out.println("|        ||");
			System.out.println("|________||");
			break;
		case 5:
			System.out.print("    _______\n");
			System.out.println("|   |    ||");
			System.out.println("|  \\O/   ||");
			System.out.println("|   |    ||");

			for (int i = 0; i < 3; i++)
				System.out.println("|        ||");
			System.out.println("|________||");
			break;
		case 6:
			System.out.print("    _______\n");
			System.out.println("|   |    ||");
			System.out.println("|  \\O/   ||");
			System.out.println("|   |    ||");
			System.out.println("|  / \\   ||");

			for (int i = 0; i < 2; i++)
				System.out.println("|        ||");
			System.out.println("|________||");
			break;
		default:
			System.out.println("ERROR: EXCEEDED WRONG NUMBER OF GUESSES!");
		}

		System.out.println("\n");
	}

	private static void getPlayerGuess(Scanner keyboard, String randomWord, List<Character> playerGuesses) {
        int incorrectGuesses = 0;
        while (incorrectGuesses < 6) {
            System.out.println("GUESS A CHARACTER!:");
            String letterGuess = keyboard.nextLine(); 
            if (!randomWord.contains(letterGuess)) {
                incorrectGuesses++;
                System.out.println("Incorrect! " + (6 - incorrectGuesses) + " incorrect guesses left!");
                displayHangMan(incorrectGuesses); 
            }
            playerGuesses.add(letterGuess.charAt(0));
            if (printWordState(randomWord, playerGuesses)) {
                System.out.println("Congratulations, you have guessed the word correctly!");
                return;
            }
            if (incorrectGuesses == 6) {
                System.out.println("Game over, you lost!\n The Word was " + randomWord + ".\n Better luck next time.");
                return;
            }
        }
    }

    public static String getRandomWord(List<String> words, int len) {
        Random rand = new Random();
        String word = "";
        do {
            word = words.get(rand.nextInt(words.size()));
        } while (word.length() < len);
        return word;
    }

    private static boolean printWordState(String word, List<Character> playerGuesses) {
        int correctCount = 0;
        for (int i = 0; i < word.length(); i++) { 
            if (playerGuesses.contains(word.charAt(i))) {
                System.out.print(word.charAt(i));
                correctCount++;
            } else {
                System.out.print("_ ");
            }
        }
        System.out.println();
        return (word.length() == correctCount);
    }
}
