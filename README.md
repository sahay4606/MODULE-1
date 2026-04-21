A simple dice betting game built using Dart and Flutter.

🚀 Features

* Start with a wallet balance of 10 coins
* Place a wager based on your balance
* Choose a game type:
    * 2 Alike
    * 3 Alike
    * 4 Alike
* Roll 4 dice randomly
* Win if at least the selected number of dice match

💰 Game Logic

* If you win → gain wager × multiplier coins
* If you lose → lose wager × multiplier coins
* Maximum wager depends on your current balance and selected multiplier

🧠 How It Works

* Generates 4 random dice values (1–6)
* Counts occurrences of each number
* Checks if any number appears 2, 3, or 4 times (based on selection)

📱 Tech Stack

* Flutter (UI)
* Dart (logic)
* Random number generation for dice simulation
