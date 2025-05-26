# Exerc-cio-01---Alef-o-Sapo-Perdido-no-Labirinto-com-T-neis-
import random
from collections import deque

def resolver_labirinto(n, m, k, maze, tunnels, simulacoes=1000):
    
    start = None
    for i in range(n):
        for j in range(m):
            if maze[i][j] == 'A':
                start = (i, j)
                break
        if start is not None:
            break

    
    tunel_dict = {}
    for i1, j1, i2, j2 in tunnels:
        tunel_dict[(i1, j1)] = (i2, j2)
        tunel_dict[(i2, j2)] = (i1, j1)

    
    moves = [(-1, 0), (1, 0), (0, -1), (0, 1)]

   
    saidas = set()
    for i in range(n):
        for j in range(m):
            if (i == 0 or i == n-1 or j == 0 or j == m-1) and maze[i][j] != '#':
                saidas.add((i, j))

    if not saidas:
        return 0.0

  
    visitado = set([start])
    fila = deque([start])

    while fila:
        x, y = fila.popleft()

       
        for dx, dy in moves:
            nx, ny = x + dx, y + dy
            if 0 <= nx < n and 0 <= ny < m:
                if maze[nx][ny] != '#' and (nx, ny) not in visitado:
                    visitado.add((nx, ny))
                    fila.append((nx, ny))

       
        if (x, y) in tunel_dict:
            nx, ny = tunel_dict[(x, y)]
            if maze[nx][ny] != '#' and (nx, ny) not in visitado:
                visitado.add((nx, ny))
                fila.append((nx, ny))

    # Contar quantas saídas são alcançadas
    saidas_alcancaveis = sum(1 for s in saidas if s in visitado)

    probabilidade = saidas_alcancaveis / len(saidas)
    return round(probabilidade, 2)


def gerar_labirinto(n, m):
    elementos = ['O'] * 5 + ['#', '*', '%']
    labirinto = [''.join(random.choice(elementos) for _ in range(m)) for _ in range(n)]
    i, j = random.randint(0, n-1), random.randint(0, m-1)
    linha = list(labirinto[i])
    linha[j] = 'A'
    labirinto[i] = ''.join(linha)
    return labirinto

def gerar_tuneis(k, n, m):
    tuneis = set()
    while len(tuneis) < k:
        i1, j1 = random.randint(0, n-1), random.randint(0, m-1)
        i2, j2 = random.randint(0, n-1), random.randint(0, m-1)
        if (i1 != i2 or j1 != j2):
            tuneis.add((i1, j1, i2, j2))
    return list(tuneis)

def entrada_manual():
    n, m, k = map(int, input().split())
    maze = [input() for _ in range(n)]
    tunnels = [tuple(map(int, input().split())) for _ in range(k)]
    return n, m, k, maze, tunnels

def main():
    n, m, k, maze, tunnels = entrada_manual()
    resultado = resolver_labirinto(n, m, k, maze, tunnels)
    print(resultado)

if __name__ == "__main__":
    main()
