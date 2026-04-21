import 'package:flutter/material.dart';
import 'dart:math';

void main() {
  runApp(const DiceGameApp());
}

class DiceGameApp extends StatelessWidget {
  const DiceGameApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      title: 'Dice Game',
      home: DiceGameScreen(),
    );
  }
}

class DiceGameScreen extends StatefulWidget {
  const DiceGameScreen({super.key});

  @override
  State<DiceGameScreen> createState() => _DiceGameScreenState();
}

class _DiceGameScreenState extends State<DiceGameScreen> {
  int walletBalance = 10;
  final TextEditingController wagerController = TextEditingController();

  String selectedGameType = "2 Alike";
  String gameResult = "";

  void rollDiceAndPlay() {
    int wager = int.tryParse(wagerController.text) ?? 0;
    int maxWager = calculateMaxWager();

    if (wager <= 0 || wager > maxWager) {
      setState(() {
        gameResult = "Invalid wager. Maximum allowed: $maxWager coins.";
      });
      return;
    }

    List<int> dice = List.generate(4, (_) => Random().nextInt(6) + 1);

    Map<int, int> diceCounts = {};
    for (int die in dice) {
      diceCounts[die] = (diceCounts[die] ?? 0) + 1;
    }

    int multiplier = int.parse(selectedGameType[0]);
    bool won = diceCounts.containsValue(multiplier);

    setState(() {
      if (won) {
        walletBalance += wager * multiplier;
        gameResult =
            "You rolled: $dice\nYou won ${wager * multiplier} coins!";
      } else {
        walletBalance -= wager * multiplier;
        gameResult =
            "You rolled: $dice\nYou lost ${wager * multiplier} coins.";
      }

      wagerController.clear();
    });
  }

  int calculateMaxWager() {
    int multiplier = int.parse(selectedGameType[0]);
    return walletBalance ~/ multiplier;
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Dice Game')),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              "Wallet Balance: $walletBalance coins",
              style: const TextStyle(
                  fontSize: 20, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 20),

            TextField(
              controller: wagerController,
              keyboardType: TextInputType.number,
              decoration: const InputDecoration(
                labelText: "Enter Wager",
                border: OutlineInputBorder(),
              ),
            ),
            const SizedBox(height: 20),

            DropdownButton<String>(
              value: selectedGameType,
              items: ["2 Alike", "3 Alike", "4 Alike"]
                  .map((type) => DropdownMenuItem(
                        value: type,
                        child: Text(type),
                      ))
                  .toList(),
              onChanged: (value) {
                setState(() {
                  selectedGameType = value!;
                });
              },
            ),
            const SizedBox(height: 20),

            ElevatedButton(
              onPressed: rollDiceAndPlay,
              child: const Text("Go"),
            ),
            const SizedBox(height: 20),

            Text(
              gameResult,
              style: const TextStyle(
                  fontSize: 18, color: Colors.blueAccent),
            ),
          ],
        ),
      ),
    );
  }
}
