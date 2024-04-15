def truth_table(expression):
    variables = set(expression)
    variables = sorted(variables)
    n = len(variables)

    # Generate all possible combinations of truth values for variables
    truth_values = []
    for i in range(2 ** n):
        values = []
        for j in range(n):
            values.append((i >> j) & 1)
        truth_values.append(values)

    # Evaluate the expression for each combination of truth values
    table = []
    for values in truth_values:
        row = values + [eval(expression, dict(zip(variables, values)))]
        table.append(row)

    return variables + ["Output"], table

def canonical_forms(truth_table):
    variables = truth_table[0][:-1]
    rows = truth_table[1]

    minterms = [i for i, row in enumerate(rows) if row[-1] == 1]
    maxterms = [i for i, row in enumerate(rows) if row[-1] == 0]

    minterm_expressions = []
    for minterm in minterms:
        minterm_expression = ""
        for i, value in enumerate(rows[minterm][:-1]):
            if value == 1:
                minterm_expression += variables[i]
            else:
                minterm_expression += f"not({variables[i]})"
            if i != len(variables) - 1:
                minterm_expression += " AND "
        minterm_expressions.append(minterm_expression)

    maxterm_expressions = []
    for maxterm in maxterms:
        maxterm_expression = ""
        for i, value in enumerate(rows[maxterm][:-1]):
            if value == 0:
                maxterm_expression += variables[i]
            else:
                maxterm_expression += f"not({variables[i]})"
            if i != len(variables) - 1:
                maxterm_expression += " OR "
        maxterm_expressions.append(maxterm_expression)

    return minterm_expressions, maxterm_expressions

def main():
    expression = input("Entrez l'expression logique en utilisant les opérateurs logiques (&, |, ^, ~) et les variables (A, B, C, ...) : ")
    truth_table_result = truth_table(expression)
    print("\nTable de vérité :")
    for row in truth_table_result[1]:
        print(row)

    canonical_forms_result = canonical_forms(truth_table_result)
    print("\nPremière forme canonique (somme de produits) :")
    for expression in canonical_forms_result[0]:
        print(expression)
    print("\nDeuxième forme canonique (produit de sommes) :")
    for expression in canonical_forms_result[1]:
        print(expression)

if __name__ == "__main__":
    main()
