# Black_Jack_Attempt
Poor BlkJkPrg
// crm-blackjack.cpp : Defines the entry point for the console application.
//
#include "stdafx.h"
#include <iostream>
#include <ctime>
#include "deck.h"
#include "Player.h"
using namespace std;

char char_choices(char chap_1) {
	char cha_choice_tmp = chap_1;
	while (cha_choice_tmp != 's' && cha_choice_tmp != 'S' && cha_choice_tmp != 'h' && cha_choice_tmp != 'H'){
		cout << "Options are (H)it or (S)tay:";
		cin >> cha_choice_tmp;
	}
	return cha_choice_tmp;
}

void hit_or_stay(Player& player1, vector<int>& vint_deck, int& int_card) {
	char char_choice = ' ';
	while ((get_hand_value(player1.get_hand())) < 21 && char_choice != 's' && char_choice != 'S' && player1.get_hand_size() != 5) {
		cout << player1.get_name() << " (H)it or (S)tay: " << endl;
		cin >> char_choice;
		char_choices(char_choice);
		if (char_choice == 'h' || char_choice == 'H') {
			player1.add_card(vint_deck.at(int_card++));
		}
		cout << player1.get_name() << "'s hand is: " << endl;
		print_hand(player1.get_hand());
		cout << "Card Total: " << get_hand_value(player1.get_hand()) << endl;
	}
	//cerr << int_card << endl;
	if (get_hand_value(player1.get_hand()) > 21)
		cout << player1.get_name() << " busts! Too bad." << endl;
}

void dealer_hs(Player& dealer, vector<int>& vint_deck, int& int_card) {
	while ((get_hand_value(dealer.get_hand())) < 16 && dealer.get_hand_size() !=5) {
		cout << "Dealer Hits." << endl;
		dealer.add_card(vint_deck.at(int_card++));
		if (get_hand_value(dealer.get_hand()) > 21)
			cout << dealer.get_name() << " busts! Too bad." << endl;
	}
	cout << endl;
	cout << dealer.get_name() << "'s hand is: " << endl;
	print_hand(dealer.get_hand());
	cout << "Card Total: " << get_hand_value(dealer.get_hand()) << endl;
	cout << "Dealer stays." << endl;
}


void gameplay(Player& ply_who, vector<int>& vtr_deck, int& int_card) {
	cout << ply_who.get_name() << "'s hand is: " << endl;
	print_hand(ply_who.get_hand());
	cout << "Card Total: " << get_hand_value(ply_who.get_hand()) << endl;
	ply_who.input_bet();
	ply_who.set_balance((ply_who.get_balance()) - (ply_who.get_bet()));
	cout << "You bet: " << ply_who.get_bet() << endl;
	hit_or_stay(ply_who, vtr_deck, int_card);
}


void dealer_gameplay(Player& dealer, vector<int>& vint_deck, int& int_card) {
	cout << dealer.get_name() << "'s hand is: " << endl;
	print_hand(dealer.get_hand());
	cout << "Card Total: " << get_hand_value(dealer.get_hand()) << endl;
	dealer.set_balance((dealer.get_balance()) - (dealer.get_dealer_bet()));
	cout << "Dealer bets: " << dealer.get_dealer_bet() << "." << endl;
	dealer_hs(dealer, vint_deck, int_card);
}



int _tmain(int argc, _TCHAR* argv[])
{
	srand((unsigned)time(NULL));
	int int_card = 0; //0 + 51
	int int_min_bet = 100;
	vector<int> vint_deck = create_deck(1);
	vector<Player> vply_players;
	Player dealer = Player("Dealer", 1000);
	Player player1 = Player("Player 1", 1000);
	Player player2 = Player("Player 2", 1000);
/*
	dealer.set_dealer_bet(int_min_bet);
	player1.set_min_bet(int_min_bet);
	player2.set_min_bet(int_min_bet);
	vint_deck = shuffle(vint_deck);

	for (int int_deal_two = 0; int_deal_two < 2; int_deal_two++) {
		dealer.add_card(vint_deck.at(int_card++));
		player1.add_card(vint_deck.at(int_card++));
		player2.add_card(vint_deck.at(int_card++));
	}
	
	cout << "Welcome to Blackjack." << endl;

	gameplay(player1, vint_deck, int_card);

	cout << player1.get_name() << "'s balance: " << player1.get_balance() << " Bet: " << player1.get_bet() << endl;

	cout << endl;

	gameplay(player2, vint_deck, int_card);
	cout << player2.get_name() << "'s balance: " << player2.get_balance() << " Bet: " << player2.get_bet() << endl;

	cout << endl;

	dealer_gameplay(dealer, vint_deck, int_card);
	cout << dealer.get_name() << "'s balance: " << dealer.get_balance() << " Bet: " << dealer.get_dealer_bet() << endl;
	
	vector<Player> vtr_plys;
	vtr_plys.push_back(player1);
	vtr_plys.push_back(player2);
	//vtr_plys.push_back(dealer);
	*/
	
	
	vector<int> vtr_test_hand{ 2, 2, 2, };
	vector<int> vtr_test_hand2{ 3, 3, 3, };
	vector<int> vtr_test_hand3{ 5, 10, 10, };

	player1.set_hand(vtr_test_hand);
	player2.set_hand(vtr_test_hand);
	dealer.set_hand(vtr_test_hand3);
	player1.set_test_bet(100);
	player2.set_test_bet(100);
	dealer.set_dealer_bet(100);


	int int_add_to = 0;
	if ((dealer.getHandValue(dealer)) >= (player1.getHandValue(player1)) && (dealer.getHandValue(dealer)) >= (player2.getHandValue(player2))) {
		int_add_to = (player1.get_bet()) + player2.get_bet() + dealer.get_dealer_bet();
		dealer.increase_balance(int_add_to);
		cout << dealer.get_name() << " wins." << endl;
		cout << dealer.get_name() << " gets the pot." << " Gains " << int_add_to << "." << endl <<
			"New balance: " << dealer.get_balance() << endl;
	}
	if ((dealer.getHandValue(dealer)) < (player1.getHandValue(player1)) && (dealer.getHandValue(dealer)) < (player2.getHandValue(player2))) {
		if ((player1.getHandValue(player1)) == (player2.getHandValue(player2))) {
			int_add_to = ((player1.get_bet() + player2.get_bet() + dealer.get_dealer_bet()) / 2);
			player1.increase_balance(int_add_to);
			player2.increase_balance(int_add_to);
			cout << player1.get_name() << " & " << player2.get_name() << " tied." << endl;
			cout << player1.get_name() << " & " << player2.get_name() << " split the pot." << endl
				<< "Gains " << int_add_to << "." << endl <<
				"New balance: " << player1.get_name() << ": " << player1.get_balance() << ". " << player2.get_name() << ": " << player2.get_balance() << "." << endl;
		}
		else if ((player1.getHandValue(player1)) > (player2.getHandValue(player2))) {
			int_add_to = (player1.get_bet() + player2.get_bet() + dealer.get_dealer_bet());
			player1.increase_balance(int_add_to);
			cout << player1.get_name() << " wins." << endl;
			cout << player1.get_name() << " gets the pot." << " Gains " << int_add_to << "." << endl <<
				"New balance: " << player1.get_balance() << endl;
		}
		else {
			int_add_to = (player1.get_bet() + player2.get_bet() + dealer.get_dealer_bet());
			player2.increase_balance(int_add_to);
			cout << player2.get_name() << " wins." << endl;
			cout << player2.get_name() << " gets the pot." << " Gains " << int_add_to << "." << endl <<
				"New balance: " << player2.get_balance() << endl;
		}
	}
	return 0;
}
