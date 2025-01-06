# MODULE-1
DEVSOC MODULE 1 











import 'package:flutter/material.dart';
import 'dart:math';

void main() {
  runApp(DiceGameApp());
}

class DiceGameApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Dice Game',
      home: DiceGameScreen(),
    );
  }
}

class DiceGameScreen extends StatefulWidget {
  @override
  _DiceGameScreenState createState() => _DiceGameScreenState();
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
        "You rolled: $dice\nCongratulations! You won ${wager * multiplier} coins.";
      } else {
        walletBalance -= wager * multiplier;
        gameResult =
        "You rolled: $dice\nSorry! You lost ${wager * multiplier} coins.";
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
      appBar: AppBar(
        title: Text('Dice Game'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text("Wallet Balance: $walletBalance coins",
                style: TextStyle(fontSize: 20, fontWeight: FontWeight.bold)),
            SizedBox(height: 20),
            TextField(
              controller: wagerController,
              keyboardType: TextInputType.number,
              decoration: InputDecoration(
                labelText: "Enter Wager",
                border: OutlineInputBorder(),
              ),
            ),
            SizedBox(height: 20),
            DropdownButton<String>(
              value: selectedGameType,
              items: ["2 Alike", "3 Alike", "4 Alike"]
                  .map((type) => DropdownMenuItem(
                child: Text(type),
                value: type,
              ))
                  .toList(),
              onChanged: (value) {
                setState(() {
                  selectedGameType = value!;
                });
              },
            ),
            SizedBox(height: 20),
            ElevatedButton(
              onPressed: rollDiceAndPlay,
              child: Text("Go"),
            ),
            SizedBox(height: 20),
            Text(
              gameResult,
              style: TextStyle(fontSize: 18, color: Colors.blueAccent),
            ),
          ],
        ),
      ),
    );
  }
}
