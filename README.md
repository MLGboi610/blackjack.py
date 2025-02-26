# blackjack.py
import random

# Card deck setup
suits = ['Hearts', 'Diamonds', 'Clubs', 'Spades']
ranks = ['2', '3', '4', '5', '6', '7', '8', '9', '10', 'Jack', 'Queen', 'King', 'Ace']
values = {'2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9, '10': 10,
          'Jack': 10, 'Queen': 10, 'King': 10, 'Ace': 11}

def create_deck():
    return [{'suit': suit, 'rank': rank, 'value': values[rank]} for suit in suits for rank in ranks]

def deal_card(deck):
    return deck.pop(random.randint(0, len(deck) - 1))

def calculate_score(hand):
    score = sum(card['value'] for card in hand)
    num_aces = sum(1 for card in hand if card['rank'] == 'Ace')
    
    while score > 21 and num_aces:
        score -= 10  # Convert Ace from 11 to 1
        num_aces -= 1
    return score

def display_hand(player, hand):
    print(f"{player}'s hand: " + ", ".join(f"{card['rank']} of {card['suit']}" for card in hand))

def blackjack():
    deck = create_deck()
    random.shuffle(deck)
    player_hand = [deal_card(deck), deal_card(deck)]
    dealer_hand = [deal_card(deck), deal_card(deck)]
    
    while True:
        display_hand('Player', player_hand)
        print(f"Player's score: {calculate_score(player_hand)}")
        
        if calculate_score(player_hand) == 21:
            print("Blackjack! You win!")
            return
        elif calculate_score(player_hand) > 21:
            print("Bust! You lose!")
            return
        
        choice = input("Do you want to hit or stand? (h/s): ").lower()
        if choice == 'h':
            player_hand.append(deal_card(deck))
        elif choice == 's':
            break
    
    print("\nDealer's turn...")
    display_hand('Dealer', dealer_hand)
    
    while calculate_score(dealer_hand) < 17:
        dealer_hand.append(deal_card(deck))
        display_hand('Dealer', dealer_hand)
        
    player_score = calculate_score(player_hand)
    dealer_score = calculate_score(dealer_hand)
    print(f"\nFinal Scores - Player: {player_score}, Dealer: {dealer_score}")
    
    if dealer_score > 21 or player_score > dealer_score:
        print("You win!")
    elif player_score < dealer_score:
        print("Dealer wins!")
    else:
        print("It's a tie!")

if __name__ == "__main__":
    blackjack()
