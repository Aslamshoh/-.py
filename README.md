import tkinter as tk
from tkinter import messagebox


class TicTacToe:
    def __init__(self, root):
        self.root = root
        self.root.title("Крестики-нолики")

        # Изменение фона окна
        self.root.configure(bg="lightblue")

        self.board = [" "] * 9  # Игровое поле
        self.current_player = "X"  # Начинает игрок "X"

        # Создаем кнопки для поля 3x3
        self.buttons = []
        for i in range(9):
            button = tk.Button(self.root, text=" ", font='normal 20 bold', height=3, width=6,
                               bg="lightyellow", fg="black",  # Цвет фона и текста кнопок
                               command=lambda i=i: self.player_move(i))
            button.grid(row=i // 3, column=i % 3, padx=5, pady=5)  # Отступы между кнопками
            self.buttons.append(button)

    def player_move(self, i):
        """Обрабатывает ход игрока"""
        if self.board[i] == " " and not self.check_winner():
            self.board[i] = self.current_player
            # Изменение цвета текста в зависимости от игрока
            color = "red" if self.current_player == "X" else "blue"
            self.buttons[i].config(text=self.current_player, fg=color)
            if self.check_winner():
                self.show_winner(f"Игрок {self.current_player} выиграл!")
            elif " " not in self.board:
                self.show_winner("Ничья!")
            else:
                self.current_player = "O" if self.current_player == "X" else "X"  # Меняем игрока

    def check_winner(self):
        """Проверка на наличие победителя"""
        win_conditions = [(0, 1, 2), (3, 4, 5), (6, 7, 8),
                          (0, 3, 6), (1, 4, 7), (2, 5, 8),
                          (0, 4, 8), (2, 4, 6)]
        for condition in win_conditions:
            if self.board[condition[0]] == self.board[condition[1]] == self.board[condition[2]] != " ":
                return True
        return False

    def show_winner(self, message):
        """Показывает сообщение о победе или ничьей"""
        messagebox.showinfo("Игра окончена", message)
        self.reset_board()

    def reset_board(self):
        """Сбрасывает игровое поле для новой игры"""
        self.board = [" "] * 9
        self.current_player = "X"
        for button in self.buttons:
            button.config(text=" ", fg="black", bg="lightyellow")  # Сброс цвета кнопок


if __name__ == "__main__":
    root = tk.Tk()
    game = TicTacToe(root)
    root.mainloop()
