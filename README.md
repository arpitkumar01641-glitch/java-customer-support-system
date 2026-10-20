# java-customer-support-system
java-customer-support-system.......which can help us to having support 
import java.util.*;

public class CustomerSupportSystem {

    static class Intent {
        String name;
        List<String> keywords;
        String response;
        boolean escalate;

        Intent(String name, List<String> keywords, String response, boolean escalate) {
            this.name = name;
            this.keywords = keywords;
            this.response = response;
            this.escalate = escalate;
        }
    }

    static List<Intent> intents = new ArrayList<>();
    static int ticketCounter = 1000;

    static void initIntents() {
        intents.add(new Intent("Order Status",
                Arrays.asList("order", "track", "delivery", "shipped", "where"),
                "You can track your order from 'My Orders' using your order ID.", false));
        intents.add(new Intent("Refund",
                Arrays.asList("refund", "return", "money back", "cancel"),
                "Refunds are processed within 5-7 working days after the return is approved.", false));
        intents.add(new Intent("Password Reset",
                Arrays.asList("password", "login", "forgot", "reset", "locked"),
                "Click 'Forgot Password' on the login page and follow the OTP steps.", false));
        intents.add(new Intent("Payment Issue",
                Arrays.asList("payment", "charged", "deducted", "failed", "double"),
                "Sorry for the trouble. Your payment issue is being forwarded to a human agent.", true));
        intents.add(new Intent("Complaint",
                Arrays.asList("angry", "worst", "complaint", "manager", "legal", "fraud"),
                "We apologise. Connecting you to a senior support agent.", true));
    }

    static Intent detectIntent(String query) {
        String text = query.toLowerCase();
        Intent best = null;
        int bestScore = 0;

        for (Intent in : intents) {
            int score = 0;
            for (String k : in.keywords) {
                if (text.contains(k)) score++;
            }
            if (score > bestScore) {
                bestScore = score;
                best = in;
            }
        }
        return best;
    }

    static void handleQuery(String query) {
        Intent intent = detectIntent(query);

        if (intent == null) {
            int id = ++ticketCounter;
            System.out.println("Bot: Sorry, I couldn't understand that. Ticket #" + id + " created for a human agent.\n");
            return;
        }

        System.out.println("Bot [" + intent.name + "]: " + intent.response);
        if (intent.escalate) {
            int id = ++ticketCounter;
            System.out.println("Bot: Escalated. Ticket #" + id + " created.");
        }
        System.out.println();
    }

    public static void main(String[] args) {
        initIntents();
        Scanner sc = new Scanner(System.in);

        System.out.println("=== AI Customer Support (type 'exit' to quit) ===\n");
        while (true) {
            System.out.print("You: ");
            String query = sc.nextLine();
            if (query.equalsIgnoreCase("exit")) break;
            handleQuery(query);
        }
    }
}
