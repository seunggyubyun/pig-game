/*
GAME RULES:

- The game has 2 players, playing in rounds
- In each turn, a player rolls a dice1 as many times as he whishes. Each result get added to his ROUND score
- BUT, if the player rolls a 1, all his ROUND score gets lost. After that, it's the next player's turn
- The player can choose to 'Hold', which means that his ROUND score gets added to his GLBAL score. After that, it's the next player's turn
- The first player to reach 100 points on GLOBAL score wins the game


*/
var scores, roundScore, activePlayer, gamePlaying;

init();

//document.querySelector('#current-' + activePlayer).textContent = dice1;
//putting html into the element using innerHTML method
//document.querySelector('#current-' + activePlayer).innerHTML = '<em>' + dice1 + '</em>'

//only reading the element
//var x = document.querySelector('#score-0').textContent;

var lastDice1,lastDice2;
document.querySelector('.btn-roll').addEventListener('click', function() {
  if(gamePlaying){
    //anonymous function doesnt have a name so cannot be used like this none
    //1. Random number
    var dice1 = Math.floor(Math.random() * 6) + 1;
    var dice2 = Math.floor(Math.random() * 6) + 1;
    //2. Display result
    var dice1Dom = document.querySelector('.dice1');
    dice1Dom.style.display='block';
    dice1Dom.src = 'dice-' + dice1 + '.png';

    var dice2Dom = document.querySelector('.dice2');
    dice2Dom.style.display='block'
    dice2Dom.src= 'dice-' + dice2 + '.png';

    if (lastDice1 === 6 && dice1 ===6 || lastDice2 === 6 && dice2 ===6){
      scores[activePlayer] = 0
      document.querySelector('#score-' + activePlayer).textContent = '0';
      lastDice1 = -1
      lastDice2 = -1
      nextPlayer();
    }
    //update the round score IF the rolled number was NOT a 1
    else if (dice1 !== 1 && dice2 !== 1){
      //Add score
      roundScore += dice1 + dice2;
      document.querySelector('#current-' + activePlayer).textContent = roundScore;
      lastDice1 = dice1;
      lastDice2 = dice2;
    } else{
      //Next player
      lastDice1 = -1
      lastDice2 = -1
      nextPlayer();
    }


  }

})

document.querySelector('.btn-hold').addEventListener('click', function() {
  if(gamePlaying){
    //Add CURRENT score to GLOBAL score
    scores[activePlayer] += roundScore;

    //Update the UI
    document.querySelector('#score-' + activePlayer).textContent = scores[activePlayer];

    var input = document.querySelector('.final-score').value;
    var winningScore;
    //Undefined, 0, null or "" are COERCED to false
    //Anything else is COERCED to true
    if(input) {
      winningScore = input;
    } else {
      winningScore = 100;
    }
    //Check if player won the game
    if (scores[activePlayer] >= winningScore){
      document.querySelector('#name-' + activePlayer).textContent = 'Winner!';
      document.querySelector('.dice1').style.display = 'none';
      document.querySelector('.player-' + activePlayer + '-panel').classList.add('winner');
      document.querySelector('.player-' + activePlayer + '-panel').classList.remove('active');
      gamePlaying = false;
    } else {
      //nextPlayer
      nextPlayer();
    }
  }
});

function nextPlayer() {

  //tenary operator if A == 0 then A = 1 else A = 0
  activePlayer === 0 ? activePlayer = 1 : activePlayer = 0;
  roundScore = 0;

  document.getElementById('current-0').textContent = '0'
  document.getElementById('current-1').textContent = '0'

  //toggle removes if it has and adds if it doesnt have
  document.querySelector('.player-0-panel').classList.toggle('active');
  document.querySelector('.player-1-panel').classList.toggle('active');

  //html class active being removed and added to player when 1 is rolled
  //document.querySelector('.player-0-panel').classList.remove('active');
  //document.querySelector('.player-1-panel').classList.add('active');

  document.querySelector('.dice1').style.disply = 'none';
}

document.querySelector('.btn-new').addEventListener('click', init);

function init() {
  scores = [0,0];
  activePlayer = 0;
  roundScore = 0;
  gamePlaying = true;

  //changing the css of the element display is css property just like margin, padding
  document.querySelector('.dice1').style.display = 'none';
  document.querySelector('.dice2').style.display = 'none';

  //different way to select elements beside querySelector
  document.getElementById('score-0').textContent = '0';
  document.getElementById('score-1').textContent = '0';
  document.getElementById('current-0').textContent = '0';
  document.getElementById('current-1').textContent = '0';
  document.getElementById('name-0').textContent = 'Player 1'
  document.getElementById('name-1').textContent = 'Player 2'
  document.querySelector('.player-0-panel').classList.remove('winner');
  document.querySelector('.player-1-panel').classList.remove('winner');

  document.querySelector('.player-0-panel').classList.remove('active');
  document.querySelector('.player-1-panel').classList.remove('active');

  document.querySelector('.player-0-panel').classList.add('active');


}
