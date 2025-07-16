def find_goppa_poly_no_roots(m=6, t=2):
    print(f"Searching for G(x) ∈ GF(2)[x] of degree {t} with no roots in GF(2^{m})")
    
    F_ext = GF(2^m, name='a')
    R.<x> = GF(2)[]
    
    candidates = [g for g in R.polynomials(of_degree=t) if g.is_irreducible()]
    list_g=[]
    for g in candidates:
        if all(F_ext(g(α)) != 0 for α in F_ext):
            print("Found suitable g(x):", g)
            list_g.append(g)
            #return g
    if list_g is not None:
        return list_g
    print("No irreducible polynomial found without roots in GF(2^m).")
    return None


def check_common_codeword_binary_goppa(m=6, t=2):
    import random

    # Define extension field GF(2^m)
    F_ext = GF(2^m, name='a')

    # Step 1: Find g(x) ∈ GF(2)[x] with no roots in GF(2^m)
    g_bin_l = find_goppa_poly_no_roots(m, t)
    if g_bin_l is None:
        print("Aborting: No valid g(x) found.")
        return False
    for g_bin in g_bin_l:
        print("experiment with g(x): ", g_bin)
        # Step 2: Convert g(x) to GF(2^m)[x] for compatibility with evaluation points
        R_ext.<x> = F_ext[]
        g = R_ext(g_bin)

        # Step 3: Use full GF(2^m) as set L since g(α) ≠ 0 for all α
        L = list(F_ext)
        random.shuffle(L)
        A = L[:len(L)//2]
        B = L[len(L)//2:]
        print(f"Set sizes: A = {len(A)}, B = {len(B)}")

        # Step 4: Construct Goppa codes
        C_A = codes.GoppaCode(g, A)
        C_B = codes.GoppaCode(g, B)

        H_A = C_A.parity_check_matrix()
        H_B = C_B.parity_check_matrix()

        # Step 5: Compute kernel intersections
        K_A = H_A.right_kernel()
        K_B = H_B.right_kernel()
        #print(H_A)
        #print('K_A:' ,K_A)
        common = K_A.intersection(K_B)
        count = 0
        if common.dimension() > 0:
            print(A)
            print(B)
            print(f"Found {common.dimension()} non-zero vector(s) a = b satisfying both syndromes.")
            for v in common.basis():
                print("→ a =", v)
            
                # Check again
                S_A = sum([v[i] / (x - A[i]) for i in range(len(A)) if v[i] != 0]).mod(g)
                S_B = sum([v[i] / (x - B[i]) for i in range(len(B)) if v[i] != 0]).mod(g)
                count +=1
                print("   Syndrome over A:", S_A == 0)
                print("   Syndrome over B:", S_B == 0)
            #return True
        else:
            print("No vector a = b satisfies both parity check conditions.")
            #return False
        if count !=0:
            print("count: ", count)
for i in range (1,18):
    print("i: ", i)
    check_common_codeword_binary_goppa(m=8, t=i)
    print("-----------------------------------------------------------------------------------------------------------------------")
