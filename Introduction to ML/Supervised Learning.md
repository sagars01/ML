

## Exercises


A] Write a program to find S and G of a given dataset

```
import itertools

def find_s_and_g(training_set):
    # Initialize S to the most specific hypothesis
    s = training_set[0][0]
    
    # Initialize G to the most general hypothesis
    g = ['?' for _ in range(len(s))]
    
    for instance, label in training_set:
        if label:  # Positive example
            # Update S
            s = [s[i] if s[i] == instance[i] else '?' for i in range(len(s))]
            
            # Update G
            g = [g[i] if g[i] == s[i] or g[i] == '?' else s[i] for i in range(len(g))]
        else:  # Negative example
            # Update G
            new_g = []
            for hypothesis in generate_hypotheses(g, instance):
                if not is_more_general(hypothesis, s):
                    new_g.append(hypothesis)
            g = new_g
    
    return s, g

def generate_hypotheses(g, instance):
    hypotheses = []
    for i in range(len(g)):
        if g[i] == '?' and instance[i] != '?':
            new_hypothesis = g.copy()
            new_hypothesis[i] = instance[i]
            hypotheses.append(new_hypothesis)
    return hypotheses

def is_more_general(h1, h2):
    return all(a == b or a == '?' for a, b in zip(h1, h2))

# Example usage
training_set = [
    (['Sunny', 'Warm', 'Normal', 'Strong', 'Warm', 'Same'], True),
    (['Sunny', 'Warm', 'High', 'Strong', 'Warm', 'Same'], True),
    (['Rainy', 'Cold', 'High', 'Strong', 'Warm', 'Change'], False),
    (['Sunny', 'Warm', 'High', 'Strong', 'Cool', 'Change'], True)
]

s, g = find_s_and_g(training_set)
print("S (most specific hypothesis):", s)
print("G (most general hypotheses):", g)

```