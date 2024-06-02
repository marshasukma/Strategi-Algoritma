import streamlit as st
from timeit import default_timer as timer
import matplotlib.pyplot as plt
import pandas as pd

# Brute Force 
def solveBr(maze, x, y, jalur, dikunjungi, kandidatSolusi, N):
    if maze[x][y] == 0 or (x == N-1 and y == N-1):
        if x == N-1 and y == N-1:
            jalur.append((x, y))
            kandidatSolusi.append(jalur.copy())
            jalur.pop()
        else:
            kandidatSolusi.append(jalur.copy())
        return

    if not validCek(x, y, N) or dikunjungi[x][y] or maze[x][y] == 0:
        return
    
    dikunjungi[x][y] = True
    jalur.append((x, y))

    for i in range(1, maze[x][y] + 1):
        if validCek(x + i, y, N):
            solveBr(maze, x + i, y, jalur, dikunjungi, kandidatSolusi, N)
        if validCek(x, y + i, N):
            solveBr(maze, x, y + i, jalur, dikunjungi, kandidatSolusi, N)
    
    jalur.pop()
    dikunjungi[x][y] = False

def validCek(x, y, N):
    return 0 <= x < N and 0 <= y < N

def solveBrBruteForce(maze, N):
    dikunjungi = [[False for _ in range(N)] for _ in range(N)]
    semuaJalur = []
    solveBr(maze, 0, 0, [], dikunjungi, semuaJalur, N)

    matrisSemuaJalur = []
    for jalur in semuaJalur:
        jalur_matrix = [[0 for _ in range(N)] for _ in range(N)]
        for (i, j) in jalur:
            jalur_matrix[i][j] = 1
        matrisSemuaJalur.append(jalur_matrix)
    return matrisSemuaJalur

def cetakKemungkinanSolusiBrute(solusi, N):
    solusi_str = ""
    for row in solusi:
        solusi_str += " ".join(map(str, row)) + "\n"
    return solusi_str

def solusiTerpendekBrute(semuaSolusi, N):
    solusiTerbaik = None
    minLangkah = float('inf')
    for solusi in semuaSolusi:
        if solusi[N-1][N-1] == 1:
            banyakLangkah = sum(sum(row) for row in solusi)
            if banyakLangkah < minLangkah:
                minLangkah = banyakLangkah
                solusiTerbaik = solusi
    return solusiTerbaik

# Backtracking Functions
def solve_RekursiBacktracking(maze, x, y, solusi):
    if x == N - 1 and y == N - 1:
        solusi[x][y] = 1
        return True

    if cekKriteriaPosisi(maze, x, y):
        solusi[x][y] = 1
        for i in range(1, maze[x][y] + 1):
            if (i <= maze[x][y]):
                if solve_RekursiBacktracking(maze, x + i, y, solusi):
                    return True
                if solve_RekursiBacktracking(maze, x, y + i, solusi):
                    return True

        solusi[x][y] = 0
        return False
    return False

def cekKriteriaPosisi(maze, x, y):
    return 0 <= x < N and 0 <= y < N and maze[x][y] != 0

def solveBacktracking(maze):
    solusi = [[0 for _ in range(N)] for _ in range(N)]
    start_time = timer()
    if not solve_RekursiBacktracking(maze, 0, 0, solusi):
        end_time = timer()
        return False, end_time - start_time, None

    end_time = timer()
    return True, end_time - start_time, solusi


st.title(" ----- Rat in Maze ----- ")
st.header(" Grab your cheese ! ")

N = st.number_input("Masukkan nilai N (ukuran matriks):", min_value=1, step=1)

if N > 0:
    maze = []
    st.write("Masukkan elemen matriks maze (baris per baris):")
    st.write("Contoh masukan : 1 0 0 1")
    for i in range(N):
        row = st.text_input(f"Baris {i+1}:")
        if row:
            row_elements = list(map(int, row.split()))
            if all(0 <= elem <= 3 for elem in row_elements) and len(row_elements) == N:
                maze.append(row_elements)
            else:
                st.error("Masukkan hanya angka 0, 1, 2, atau 3 dalam setiap baris serta jumlah masukan inputan N. Ulangi masukan.")

    st.write("your path: \n")
    maze_df = pd.DataFrame(maze)
    st.write(maze_df)

    if len(maze) == N:
        method = st.selectbox("Pilih metode penyelesaian:", ("Backtracking", "Brute Force", "Keluar"))

        if method == "Backtracking":
            hasil, time_taken, solusi = solveBacktracking(maze)
            st.write(f"Waktu untuk mencari solusi (backtracking): {time_taken:.20f} detik")
            if hasil:
                st.write("Solusi maze (backtracking):")
                s_df = pd.DataFrame(solusi)
                st.write(s_df)
            else:
                st.write("Solusi tidak ditemukan pada Backtracking.")
        
        elif method == "Brute Force":
            start_time = timer()
            hasil = solveBrBruteForce(maze, N)
            end_time = timer()

            st.write(f"Waktu brute force: {end_time - start_time:.20f} detik")
            if hasil:
                st.write("Semua kemungkinan solusi (brute force):")
                for rs in hasil:
                    st.text(cetakKemungkinanSolusiBrute(rs, N))
                
                start_time = timer()
                solusiTerbaik = solusiTerpendekBrute(hasil, N)
                end_time = timer()

                st.write(f"Waktu untuk mencari solusi terbaik: {end_time - start_time:.20f} detik")
                if solusiTerbaik:
                    st.write("Solusi terbaik yang ditemukan (brute force):")
                    sb_df = pd.DataFrame(solusiTerbaik) 
                    st.write(sb_df)
                else:
                    st.write("Tidak ada solusi yang mencapai titik [N-1][N-1] pada brute force")
            else:
                st.write("Solusi tidak ditemukan")
        
        elif method == "Keluar":
            st.write("Anda telah keluar dari aplikasi.")
