# DNA-mutation-data-analysis
import streamlit as st
import matplotlib.pyplot as plt

def detect_mutations(seq1, seq2):
    substitutions, insertions, deletions = 0, 0, 0
    i, j = 0, 0
    mutation_details = []

    while i < len(seq1) and j < len(seq2):
        if seq1[i] == seq2[j]:
            mutation_details.append((i + 1, "Match", seq1[i], seq2[j]))
            i += 1
            j += 1
        else:
            substitutions += 1
            mutation_details.append((i + 1, "Substitution", seq1[i], seq2[j]))
            i += 1
            j += 1

    # Handle insertions and deletions
    for k in range(i, len(seq1)):
        deletions += 1
        mutation_details.append((k + 1, "Deletion", seq1[k], "-"))

    for k in range(j, len(seq2)):
        insertions += 1
        mutation_details.append((len(seq1) + k + 1, "Insertion", "-", seq2[k]))

    return {"Substitutions": substitutions, "Insertions": insertions, "Deletions": deletions}, mutation_details

st.title("DNA Mutation Analysis Tool")
st.write("Enter two DNA sequences to compare and detect mutations.")

seq1 = st.text_area("Sequence 1", "ACGTACGTGAC")
seq2 = st.text_area("Sequence 2", "ACCTGCGTGAA")

if st.button("Analyze"):
    mutations, details = detect_mutations(seq1, seq2)
    st.write("### Mutation Summary")
    st.write(mutations)

    st.write("### Mutation Details")
    for detail in details:
        st.write(f"Position {detail[0]}: {detail[1]} ({detail[2]} → {detail[3]})")

    # Visualization
    st.write("### Mutation Types")
    plt.bar(mutations.keys(), mutations.values(), color='skyblue')
    plt.title("Mutation Types")
    plt.ylabel("Count")
    st.pyplot(plt)
