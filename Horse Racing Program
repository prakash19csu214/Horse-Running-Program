# Horse Racing Program

import turtle
import random
import time

def registerBetters():
    # Creat empty list to hold name of all the betters
    lst =[]
    # Instructions for the user
    print('REGISTRATION')
    print('------------')
    print('Enter names of individuals to register, hit Enter when done.')
    # Prompt first name and enter into list
    name = input('Enter name: ')
    lst.append(name)
    while name != '':
        name = input('Enter next name: ')
        lst.append(name)
    # Return the list of better names
    del lst[-1]
    print(lst)
    return lst

def getAllBets(betters, no_horses):
    name_bet_lst = []
    for i in range(0, len(betters)):
        print(betters[i], 'place your bet')
        horse = int(input('Which horse? '))
        # check to ensure users entered a valid horse number
        while horse not in range(1, no_horses + 1):
            horse = int(input('Invalid horse number, please reenter: '))
        bet = int(input('How much waging? '))
        name_bet_lst.append((horse, bet))
    print(name_bet_lst)
    return name_bet_lst

def updateGains(winner, bets, gains):
    # set up values
    total_pool = 0
    winner_bets = 0
    for i in range(0, len(bets)):
        total_pool += bets[i][1]
        if bets[i][0] == winner:
            winner_bets += bets[i][1]
    # calculate commission(15%) and remaining pool
    total_pool = total_pool * 0.85

    # calculate payout per dollar
    if winner_bets > 0:
        payout_per_dollar = total_pool / winner_bets

    # update gains list
    for i in range(0, len(bets)):
        if bets[i][0] == winner:
            gains[i] = (payout_per_dollar * bets[i][1]) - bets[i][1]
        else:
            gains[i] = gains[i] - bets[i][1]

    return gains

def getHorseImages(num_horses):
    # init empty list
    images = []
    
    # get all horse images
    for k in range(0, num_horses):
        images = images + ['horse_' + str(k+1) + '_image.gif']

    return images

def getBannerImages(num_horses):
    # init empty list
    all_images = []

    # get "They're Off" banner image
    images = ['theyre_off_banner.gif']
    all_images.append(images)
    
    # get early lead banner images
    images = []
    for k in range(0, num_horses):
        images = images + ['lead_at_start_' + str(k+1) + '.gif']
    all_images.append(images)

    # get mid-way lead banner images
    images = []
    for k in range(0, num_horses):
        images = images + ['looking_good_' + str(k+1) + '.gif']
    all_images.append(images)

    # get "We Have a Winner" banner image
    images = ['winner_banner.gif']
    all_images.append(images)

    return all_images

def registerHorseImages(images):
    for k in range(0, len(images)):
        turtle.register_shape(images[k])

def registerBannerImages(images):
    for k in range(0, len(images)):
        for j in range(0, len(images[k])):
            turtle.register_shape(images[k][j])

def newHorse(image_file):
    horse = turtle.Turtle()
    horse.hideturtle()
    horse.shape(image_file)
    
    return horse

def generateHorses(images, num_horses):
    horses = []
    for k in range(0, num_horses):
        horse = newHorse(images[k])
        horses.append(horse)

    return horses

def placeHorses(horses, loc, separation):
    for k in range(0, len(horses)):
        horses[k].hideturtle()
        horses[k].penup()
        horses[k].setposition(loc[0], loc[1] + k * separation)
        horses[k].setheading(180)
        horses[k].showturtle()
        horses[k].pendown()

def findLeadHorse(horses):
    # init
    lead_horse = 0
    
    for k in range(1, len(horses)):
        if horses[k].position()[0] < \
           horses[lead_horse].position()[0]:
            lead_horse = k
    return lead_horse

def displayBanner(banner, position):
    the_turtle = turtle.getturtle()
    turtle.setposition(position[0], position[1])
    turtle.shape(banner)
    turtle.stamp()
    
def startHorses(horses, banners, finish_line, forward_incr):
    # init
    have_winner = False
    early_leading_horse_displayed = False
    midrace_leading_horse_displayed = False
    
    # display "there're off" banner
    displayBanner(banners[0][0], (70, -300))
    
    k = 0
    while not have_winner:
        horse = horses[k]
        horse.forward(random.randint(1,3) * forward_incr)

        # display mid-race lead banner
        lead_horse = findLeadHorse(horses)
        if horses[lead_horse].position()[0] < -125 and \
           not midrace_leading_horse_displayed:
           
            displayBanner(banners[2][lead_horse], (40, -300))
            midrace_leading_horse_displayed = True

        # display early lead banner
        elif horses[lead_horse].position()[0] < 125 and \
           not early_leading_horse_displayed:
            displayBanner(banners[1][lead_horse], (10, -300))
            early_leading_horse_displayed = True
        
        # check for horse over finish line
        if horse.position()[0] < finish_line:
            have_winner = True
        else:
            k = (k+1) % len(horses)
    return k

def displayWinner(winning_horse, winner_banner):
    # display "We Have a Winner" banner
    displayBanner(winner_banner, (20, -300))

    # blink winning horse
    show = False
    counter = 10
    while counter != 0:
        if show:
            winning_horse.showturtle()
            show = False
        else:
            winning_horse.hideturtle()
            show = True

        counter = counter - 1
        time.sleep(.4)

# ---- main
def main():
    # init number of horses
    num_horses = 10

    # Get names of betters and start gains list
    betters = registerBetters()
    gains = []
    for i in range(0, len(betters)):
        gains.append(0)

    # Prompt user for the number of races they want to play
    number_of_races = int(input('Enter the number of races to run: '))

    # Loop controlling the number of races
    while number_of_races != 0:

        print('Starting a new race ... Placing bets')

        # get all the bets for this race
        bets = getAllBets(betters, num_horses)

        # set window size
        turtle.setup(750,800)

        # get turtle window
        window = turtle.Screen()

        # set window title bar
        window.title('Horse Race Simulation Program')

        # hide default turtle and keep from drawing
        turtle.hideturtle()
        turtle.penup()

        # init screen layout parameters
        start_loc = (240, -200)
        finish_line = -240
        track_separation = 60
        forward_incr = 6

        # register images
        horse_images = getHorseImages(num_horses)
        banner_images = getBannerImages(num_horses)
        registerHorseImages(horse_images)
        registerBannerImages(banner_images)

        # generate and init horses
        horses = generateHorses(horse_images, num_horses)

        # place horses at starting line
        placeHorses(horses, start_loc, track_separation)

        # start horses
        winner = startHorses(horses, banner_images, finish_line,
                             forward_incr)

        # light up for winning horse
        displayWinner(horses[winner], banner_images[3][0])

        # added 1 to winner, before it was an index value
        print('\nHorse', winner + 1, 'is the winner!\n')
        print('Registered Users Gains')
        print('----------------------')
        gains = updateGains(winner + 1, bets, gains)
        for i in range(0, len(betters)):
            print(betters[i], '$', float(gains[i]))



        # terminate program when close window
        turtle.exitonclick()

        number_of_races -= 1

    # loop is done, display that all races are over
    print('\nAll races completed')

main()
