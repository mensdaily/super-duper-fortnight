# Read the qa_pairs from the file
qa_pairs = []
with open('qa_pairs.txt', 'r') as file:
    lines = file.readlines()
    for i in range(0, len(lines), 3):
        if len(lines[i].split("Context: ")) == 2:
            context = lines[i].split("Context: ")[1].strip()
        else:
            continue  # Skip this iteration if the split did not work as expected
        
        if len(lines[i+1].split("Question: ")) == 2:
            question = lines[i+1].split("Question: ")[1].strip()
        else:
            continue  # Skip this iteration if the split did not work as expected

        if len(lines[i+2].split("Answer: ")) == 2:
            answer = lines[i+2].split("Answer: ")[1].strip()
        else:
            continue  # Skip this iteration if the split did not work as expected

        qa_pairs.append((context, question, answer))