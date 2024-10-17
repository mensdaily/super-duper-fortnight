


# Read the qa_pairs from the file
qa_pairs = []
with open('qa_pairs.txt', 'r') as file:
    lines = file.readlines()
    for i in range(0, len(lines), 3):
        try:
            context = lines[i].split("Context: ")[1].strip()
            question = lines[i + 1].split("Question: ")[1].strip()
            answer = lines[i + 2].split("Answer: ")[1].strip()
            qa_pairs.append((context, question, answer))
        except IndexError as e:
            print(f"Error processing line numbers {i}, {i+1}, and {i+2}: {e}")
        except Exception as e:
            print(f"An unexpected error occurred at lines {i}, {i+1}, {i+2}: {e}")