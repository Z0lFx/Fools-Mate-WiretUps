# Fools-Mate-WiretUps
Challenge inn Try Hackme


1. Code Review
I opened Inspect (F12)  Sources and found this function:

function preMoveCheck(from, to, promotion) {
  const probe = new Chess(game.fen());
  const result = probe.move({ from, to, promotion });
  if (result && probe.isCheckmate()) {
    showSystemNotice("I'll shut down your PC if you play that.");
    return false; // blocks the move before it's sent to the server
  }
  return true;
}



 choose  file  Js

Read the Code 
The issue: this check runs client-side only (in the browser). If bypassed, the move goes straight through to the server with no resistance.

 Board Analysis
FEN: 6k1/5ppp/8/8/8/8/5PPP/R5K1 w - - 0 1

White rook on a1
Black king on g8, boxed in by its own pawns (no escape squares)

→ a1 → a8 = checkmate in one move (Back-Rank Mate)

 Bypassing the Check
Instead of playing with the mouse, I sent the request directly to the server via the Console:
Js

This completely bypasses preMoveCheck() since it never goes through the doMove() function that calls it.
fetch('/api/move', {
  method: 'POST',
  headers: {'Content-Type': 'application/json'},
  body: JSON.stringify({from: 'a1', to: 'a8'})
}).then(r => r.json()).then(console.log)

Json 


 Result Flag{.....}



THANKS SO MUCH from 
Pr3xco
