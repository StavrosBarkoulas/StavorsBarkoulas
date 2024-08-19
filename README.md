# Roulette wheel _ Martingale Strategy _ Monte Carlo Simoulation _ Python
import random

def spin_wheel():
    """Simulates a spin of the roulette wheel. Returns 'red' or 'black'."""
    return random.choice(['red', 'black'])

def martingale_strategy(starting_bankroll, initial_bet, max_rounds):
    bankroll = starting_bankroll
    bet = initial_bet
    rounds = 0

    while rounds < max_rounds and bankroll >= bet:
        result = spin_wheel()
        
        if result == 'red':  # Assume we always bet on 'red'
            bankroll += bet  # Win: add the bet amount to bankroll
            bet = initial_bet  # Reset to initial bet
        else:
            bankroll -= bet  # Lose: subtract the bet amount from bankroll
            bet *= 2  # Double the bet for the next round

        rounds += 1

        if bankroll <= 0:
            break

    return bankroll

def monte_carlo_simulation(starting_bankroll, initial_bet, max_rounds, num_simulations):
    results = []
    for _ in range(num_simulations):
        end_bankroll = martingale_strategy(starting_bankroll, initial_bet, max_rounds)
        results.append(end_bankroll)
    return results

# Running the Monte Carlo simulation
starting_bankroll = 1000  # Initial amount of money
initial_bet = 10  # Starting bet
max_rounds = 100  # Maximum number of rounds in each simulation
num_simulations = 1000  # Number of Monte Carlo simulations

simulation_results = monte_carlo_simulation(starting_bankroll, initial_bet, max_rounds, num_simulations)

# Calculate and display average ending bankroll
average_end_bankroll = sum(simulation_results) / num_simulations
average_end_bankroll
