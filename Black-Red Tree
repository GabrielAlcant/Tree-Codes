import sys

# --- Configurações de Cores para o Terminal (Opcional, funciona em terminais ANSI) ---
class Cores:
    VERMELHO = '\033[91m'
    RESET = '\033[0m'
    NEGRITO = '\033[1m'
    VERDE = '\033[92m'

# Constantes da Árvore
RED = 1
BLACK = 0

class Node:
    def __init__(self, data):
        self.data = data
        self.parent = None
        self.left = None
        self.right = None
        self.color = RED

class RedBlackTree:
    def __init__(self):
        self.TNULL = Node(0)
        self.TNULL.color = BLACK
        self.TNULL.left = None
        self.TNULL.right = None
        self.root = self.TNULL

    # --- Lógica de Rotação e Balanceamento (Oculta para o usuário final) ---
    def left_rotate(self, x):
        y = x.right
        x.right = y.left
        if y.left != self.TNULL:
            y.left.parent = x
        y.parent = x.parent
        if x.parent is None:
            self.root = y
        elif x == x.parent.left:
            x.parent.left = y
        else:
            x.parent.right = y
        y.left = x
        x.parent = y

    def right_rotate(self, x):
        y = x.left
        x.left = y.right
        if y.right != self.TNULL:
            y.right.parent = x
        y.parent = x.parent
        if x.parent is None:
            self.root = y
        elif x == x.parent.right:
            x.parent.right = y
        else:
            x.parent.left = y
        y.right = x
        x.parent = y

    def insert(self, key):
        # Verifica duplicatas (opcional, mas recomendado para árvores de busca)
        if self.search(key) != self.TNULL:
            print(f"⚠️  O valor {key} já existe na árvore.")
            return

        node = Node(key)
        node.parent = None
        node.data = key
        node.left = self.TNULL
        node.right = self.TNULL
        node.color = RED

        y = None
        x = self.root

        while x != self.TNULL:
            y = x
            if node.data < x.data:
                x = x.left
            else:
                x = x.right

        node.parent = y
        if y is None:
            self.root = node
        elif node.data < y.data:
            y.left = node
        else:
            y.right = node

        if node.parent is None:
            node.color = BLACK
            return

        if node.parent.parent is None:
            return

        self.insert_fixup(node)
        print(f"{Cores.VERDE}✓ Elemento {key} inserido com sucesso!{Cores.RESET}")

    def insert_fixup(self, k):
        while k.parent.color == RED:
            if k.parent == k.parent.parent.right:
                u = k.parent.parent.left
                if u.color == RED:
                    u.color = BLACK
                    k.parent.color = BLACK
                    k.parent.parent.color = RED
                    k = k.parent.parent
                else:
                    if k == k.parent.left:
                        k = k.parent
                        self.right_rotate(k)
                    k.parent.color = BLACK
                    k.parent.parent.color = RED
                    self.left_rotate(k.parent.parent)
            else:
                u = k.parent.parent.right
                if u.color == RED:
                    u.color = BLACK
                    k.parent.color = BLACK
                    k.parent.parent.color = RED
                    k = k.parent.parent
                else:
                    if k == k.parent.right:
                        k = k.parent
                        self.left_rotate(k)
                    k.parent.color = BLACK
                    k.parent.parent.color = RED
                    self.right_rotate(k.parent.parent)
            if k == self.root:
                break
        self.root.color = BLACK

    def delete(self, data):
        self._delete_node_helper(self.root, data)

    def _delete_node_helper(self, node, key):
        z = self.TNULL
        while node != self.TNULL:
            if node.data == key:
                z = node
            if node.data <= key:
                node = node.right
            else:
                node = node.left

        if z == self.TNULL:
            print(f"❌ Elemento {key} não encontrado para remoção.")
            return

        y = z
        y_original_color = y.color
        if z.left == self.TNULL:
            x = z.right
            self._rb_transplant(z, z.right)
        elif z.right == self.TNULL:
            x = z.left
            self._rb_transplant(z, z.left)
        else:
            y = self._minimum(z.right)
            y_original_color = y.color
            x = y.right
            if y.parent == z:
                x.parent = y
            else:
                self._rb_transplant(y, y.right)
                y.right = z.right
                y.right.parent = y

            self._rb_transplant(z, y)
            y.left = z.left
            y.left.parent = y
            y.color = z.color

        if y_original_color == BLACK:
            self._delete_fixup(x)
        
        print(f"{Cores.VERDE}✓ Elemento {key} removido.{Cores.RESET}")

    def _delete_fixup(self, x):
        while x != self.root and x.color == BLACK:
            if x == x.parent.left:
                s = x.parent.right
                if s.color == RED:
                    s.color = BLACK
                    x.parent.color = RED
                    self.left_rotate(x.parent)
                    s = x.parent.right

                if s.left.color == BLACK and s.right.color == BLACK:
                    s.color = RED
                    x = x.parent
                else:
                    if s.right.color == BLACK:
                        s.left.color = BLACK
                        s.color = RED
                        self.right_rotate(s)
                        s = x.parent.right

                    s.color = x.parent.color
                    x.parent.color = BLACK
                    s.right.color = BLACK
                    self.left_rotate(x.parent)
                    x = self.root
            else:
                s = x.parent.left
                if s.color == RED:
                    s.color = BLACK
                    x.parent.color = RED
                    self.right_rotate(x.parent)
                    s = x.parent.left

                if s.right.color == BLACK and s.left.color == BLACK:
                    s.color = RED
                    x = x.parent
                else:
                    if s.left.color == BLACK:
                        s.right.color = BLACK
                        s.color = RED
                        self.left_rotate(s)
                        s = x.parent.left

                    s.color = x.parent.color
                    x.parent.color = BLACK
                    s.left.color = BLACK
                    self.right_rotate(x.parent)
                    x = self.root
        x.color = BLACK

    def _rb_transplant(self, u, v):
        if u.parent is None:
            self.root = v
        elif u == u.parent.left:
            u.parent.left = v
        else:
            u.parent.right = v
        v.parent = u.parent

    def _minimum(self, node):
        while node.left != self.TNULL:
            node = node.left
        return node

    def search(self, k):
        return self._search_helper(self.root, k)

    def _search_helper(self, node, key):
        if node == self.TNULL or key == node.data:
            return node
        if key < node.data:
            return self._search_helper(node.left, key)
        return self._search_helper(node.right, key)

    def print_tree(self):
        if self.root == self.TNULL:
            print("  (Árvore Vazia)")
        else:
            self._print_helper(self.root, "", True)

    def _print_helper(self, currPtr, indent, last):
        if currPtr != self.TNULL:
            sys.stdout.write(indent)
            if last:
                sys.stdout.write("R----")
                indent += "   "
            else:
                sys.stdout.write("L----")
                indent += "|  "

            s_color = f"{Cores.VERMELHO}RED{Cores.RESET}" if currPtr.color == RED else "BLACK"
            print(f"{currPtr.data} ({s_color})")
            self._print_helper(currPtr.left, indent, False)
            self._print_helper(currPtr.right, indent, True)

# --- Lógica de Interação com Usuário ---
def limpar_tela():
    print("\n" * 2)

def menu_interativo():
    arvore = RedBlackTree()
    
    while True:
        print("\n" + "="*40)
        print(f"{Cores.NEGRITO}   GERENCIADOR DE ÁRVORE RUBRO-NEGRA   {Cores.RESET}")
        print("="*40)
        print("1. Inserir Elemento")
        print("2. Remover Elemento")
        print("3. Pesquisar Elemento")
        print("4. Mostrar Árvore")
        print("5. Sair")
        print("-" * 40)
        
        opcao = input("Escolha uma opção (1-5): ")

        if opcao == '1':
            try:
                valor = int(input("Digite o número inteiro para inserir: "))
                arvore.insert(valor)
            except ValueError:
                print("❌ Entrada inválida! Por favor, digite um número inteiro.")
        
        elif opcao == '2':
            try:
                valor = int(input("Digite o número para remover: "))
                arvore.delete(valor)
            except ValueError:
                print("❌ Entrada inválida!")

        elif opcao == '3':
            try:
                valor = int(input("Digite o número para pesquisar: "))
                res = arvore.search(valor)
                if res.data != 0: # 0 é o dado do TNULL
                    cor_str = "VERMELHO" if res.color == RED else "PRETO"
                    print(f"✅ Encontrado: {res.data} | Cor: {cor_str}")
                    if res.parent:
                        print(f"   Pai: {res.parent.data}")
                    else:
                        print("   (É a Raiz)")
                else:
                    print(f"❌ Elemento {valor} não está na árvore.")
            except ValueError:
                print("❌ Entrada inválida!")

        elif opcao == '4':
            print("\nVisualização Atual da Árvore:")
            arvore.print_tree()
            input(f"\n{Cores.NEGRITO}Pressione Enter para continuar...{Cores.RESET}")

        elif opcao == '5':
            print("Encerrando programa...")
            break
        
        else:
            print("Opção inválida. Tente novamente.")
            
        # Pequena pausa para o usuário ler o resultado antes do menu voltar
        if opcao != '4' and opcao != '5':
             pass # O menu loopa direto, mas a visualização do menu ajuda a separar

if __name__ == "__main__":
    menu_interativo()
