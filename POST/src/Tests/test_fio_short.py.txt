# -*- coding: utf-8 -*-
"""
Проверка логики сокращения ФИО, совпадающей с запросом «LOAD_Сводная» (Power Query).
Правило: первый токен — фамилия; далее по каждому слову — первая буква и точка, инициалы слиты (И.И.).
"""

from __future__ import annotations

import unittest


def сократить_фио(полное: str | None) -> str:
    """Зеркало логики M-функции fnСократитьФИО для автотеста."""
    if полное is None:
        сырой = ""
    else:
        сырой = полное.strip()
    if not сырой:
        return ""
    части = [p for p in сырой.split(" ") if p.strip()]
    if not части:
        return ""
    if len(части) == 1:
        return части[0]
    фамилия = части[0]
    инициалы = "".join(f"{слово[0].upper()}." for слово in части[1:] if слово)
    return f"{фамилия} {инициалы}"


class TestСократитьФИО(unittest.TestCase):
    def test_пример_из_тз(self) -> None:
        self.assertEqual(
            сократить_фио("Иванов Иван Иванович"),
            "Иванов И.И.",
        )
        self.assertEqual(
            сократить_фио("Спиридонов Петр Ильич"),
            "Спиридонов П.И.",
        )

    def test_пусто(self) -> None:
        self.assertEqual(сократить_фио(""), "")
        self.assertEqual(сократить_фио(None), "")

    def test_одно_слово(self) -> None:
        self.assertEqual(сократить_фио("Сидоров"), "Сидоров")


if __name__ == "__main__":
    unittest.main()
