class ExamChecking extends Thread {
    private int sheetsToCheck;
    private int iterations;

    public ExamChecking(int iterations, int sheetsToCheck) {
        this.iterations = iterations;
        this.sheetsToCheck = sheetsToCheck;
    }

    @Override
    public void run() {
        for (int i = 1; i <= iterations; i++) {
            System.out.println(Thread.currentThread().getName() + " is checking sheet " + i + "/" + iterations);
            try {
                Thread.sleep(500);
            } catch (InterruptedException e) {
                System.out.println(Thread.currentThread().getName() + " was interrupted.");
            }
        }
        System.out.println(Thread.currentThread().getName() + " has finished checking " + sheetsToCheck + " sheets.");
    }
}

public class Main {
    public static void main(String[] args) {
        int totalSheets = 500;
        int sheetsPerAssistant = 50;
        int totalAssistants = totalSheets / sheetsPerAssistant;

        Thread[] assistants = new Thread[totalAssistants];

        for (int i = 0; i < totalAssistants; i++) {
            assistants[i] = new ExamChecking(6, sheetsPerAssistant);
            assistants[i].setName("Assistant-" + (i + 1));
            assistants[i].start();
        }

        for (Thread assistant : assistants) {
            try {
                assistant.join(); 
            } catch (InterruptedException e) {
                System.out.println("Main thread was interrupted.");
            }
        }
