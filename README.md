# Pobture01

import psycopg2

# Create a connection to the PostgreSQL database
def create_connection():
    try:
        conn = psycopg2.connect(
            dbname="pobture_db",
            user="nkevigrl",
            password="QWU3KqFTJF6edCDSlqRuyKQwKH6NvWnZ",
            host="rain.db.elephantsql.com"  # Or your PostgreSQL server IP address
        )
        return conn
    except psycopg2.Error as e:
        print("Error connecting to PostgreSQL:", e)
        return None

# Save scores to the PostgreSQL database
def save_scores_to_database(scores):
    conn = create_connection()
    if conn:
        try:
            with conn.cursor() as cur:
                cur.execute('''INSERT INTO personality_scores (
                                    solo_group, budget_luxury, leisure_result,
                                    digital_nomad, volunteer, ecotourist
                                ) VALUES (%s, %s, %s, %s, %s, %s)''', scores)
            conn.commit()
            print("Scores saved to the database successfully!")
        except psycopg2.Error as e:
            print("Error saving scores to PostgreSQL:", e)
        finally:
            conn.close()

def personality_test():
    print("Find your travel personality to design your ideal trip and find your travel buddy! '😍'")

    # Solo or Group rules
    choice1 = input("Are you a solo adventurer or do you prefer to travel in a group? (Solo/Group): ")
    count1 = 1 if choice1 == "Solo" else 0

    choice2 = input("Do you enjoy solo exploration or do you prefer having a companion? (Solo/Companion): ")
    count2 = 1 if choice2 == "Solo" else 0

    choice3 = input("Do you prefer to walking alone without waiting for anyone or feel more comfortable with a group? (Alone/Group): ")
    count3 = 1 if choice3 == "Alone" else 0

    total_sg = count1 + count2 + count3

    # Budget or Luxury rules
    choice4 = input("Do you plan your expenses or go with the flow? (Plan/Flow): ")
    count4 = 1 if choice4 == "Plan" else 0

    choice5 = input("Do you prioritize saving money or convenience? (Save/Convenience): ")
    count5 = 1 if choice5 == "Save" else 0

    choice6 = input("Do you prefer roadside eateries or upscale restaurants? (Roadside/Upcale): ")
    count6 = 1 if choice6 == "Roadside" else 0

    total_bl = count4 + count5 + count6

    # Leisure/Cultural or Adventure rules
    choice7 = input("Do you prefer historical sites or entertainment venues? (Historical/Entertainment): ")
    la_result = "Leisure" if choice7 == "Historical" else "Adventure"

    choice8 = input("Do you seek relaxation or excitement? (Relaxation/Excitement): ")
    la_result = "Leisure" if choice8 == "Relaxation" else la_result

    choice9 = input("Do you prefer a laid-back atmosphere or a lively one? (Laid-back/Lively): ")
    la_result = "Leisure" if choice9 == "Laid-back" else la_result

    # Digital Nomads
    choice10 = input("Are you a Digital Nomad? (Yes/No): ")
    digital_nomad = "Digital Nomad" if choice10.lower() == "yes" else ""

    # Ecotourist
    choice12 = input("Do you prefer biking over driving? (Yes/No): ")
    ecotourist = "Ecotourist" if choice12.lower() == "yes" else ""

    choice13 = input("Do you believe in preserving nature? (Yes/No): ")
    ecotourist = "Ecotourist" if choice13.lower() == "yes" else ""

# Retrieve and print the description for the result
    scores = [
        "Solo" if total_sg == 2 else "Group",
        "Budget" if total_bl == 2 else "Luxury",
        la_result,
        digital_nomad,
        ecotourist
    ]
    outcome = ", ".join(scores)
    print("You are {} Traveler.😍👍".format(outcome))
    print("Here's your personalized travel plan. You can always customize it later!")
    print("Your travel buddies are eagerly waiting for you! ╰(*°▽°*)╯")

    return scores if any(scores) else []

def save_scores(scores):
    with open('personality_scores.csv', 'a', newline='') as csvfile:
        writer = csv.writer(csvfile)
        writer.writerow(scores)

if __name__ == "__main__":
    scores = personality_test()
    if scores:
        save_scores(scores)''
