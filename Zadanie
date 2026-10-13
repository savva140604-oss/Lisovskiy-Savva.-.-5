# -*- coding: utf-8 -*-


import hashlib
import random


def sep(n, title):
    print("\n" + "-" * 62)
    print(f"ЗАДАНИЕ {n}. {title}")
    print("-" * 62)


# Задание 1. Пропускная способность записи (Т/с)
sep(1, "Пропускная способность записи (Т/с)")


def throughput(networks):
    """Возвращает {сеть: транзакций/час} и сеть-лидера."""
    per_hour = {name: tps * 3600 for name, tps in networks.items()}
    leader = max(per_hour, key=per_hour.get)
    return per_hour, leader


nets = {"Ethereum": 30, "Binance Smart Chain": 160, "Solana": 50_000}
per_hour, leader = throughput(nets)

for name, val in per_hour.items():
    print(f"{name}: {val:,} транзакций/час")
print(f"Лидер: {leader}")


# Задание 2. Соотношение операций чтения к записи
sep(2, "Соотношение операций чтения к записи")


def read_write_ratio(writes, min_ratio=100, max_ratio=10_000):
    """Возвращает (минимум чтений/с, максимум чтений/с)."""
    return writes * min_ratio, writes * max_ratio


writes = 500
reads_min, reads_max = read_write_ratio(writes)
print(f"Записей в секунду: {writes}")
print(f"Минимум чтений/с: {reads_min:,}")
print(f"Максимум чтений/с: {reads_max:,}")


# Задание 3. Простейший блокчейн из блоков
sep(3, "Простейший блокчейн из блоков")


class Block:
    def __init__(self, index, data, prev_hash):
        self.index = index
        self.data = data
        self.prev_hash = prev_hash
        self.hash = self.compute_hash()

    def compute_hash(self):
        s = str(self.index) + self.data + self.prev_hash
        return hashlib.sha256(s.encode()).hexdigest()


chain = [Block(0, "Genesis", "0")]
for i in range(1, 3):
    chain.append(Block(i, f"Блок {i}", chain[-1].hash))

for b in chain:
    print(f"Блок {b.index}: hash={b.hash[:16]}...  prev={b.prev_hash[:16]}...")


# Задание 4. Хеширование SHA-256
sep(4, "Хеширование SHA-256")


def sha256_hex(text):
    return hashlib.sha256(text.encode()).hexdigest()


print(f"SHA-256('blockchain') = {sha256_hex('blockchain')}")


# Задание 5. Proof-of-Work (майнинг)
sep(5, "Proof-of-Work (майнинг)")


def mine(data, difficulty):
    """Подбирает nonce, при котором хеш начинается с `difficulty` нулей."""
    prefix = "0" * difficulty
    nonce = 0
    while True:
        h = hashlib.sha256((data + str(nonce)).encode()).hexdigest()
        if h.startswith(prefix):
            return nonce, h
        nonce += 1


nonce, block_hash = mine("block1", 4)
print("data = 'block1', difficulty = 4")
print(f"nonce = {nonce}")
print(f"hash  = {block_hash}")


# Задание 6. Корень дерева Меркла
sep(6, "Корень дерева Меркла")


def merkle_root(transactions):
    level = [hashlib.sha256(t.encode()).hexdigest() for t in transactions]
    while len(level) > 1:
        if len(level) % 2 == 1:
            level.append(level[-1])
        level = [
            hashlib.sha256((level[i] + level[i + 1]).encode()).hexdigest()
            for i in range(0, len(level), 2)
        ]
    return level[0]


txs = ["tx1", "tx2", "tx3", "tx4"]
print(f"Merkle root: {merkle_root(txs)}")


# Задание 7. Вероятность «Атаки 51%»
sep(7, "Вероятность «Атаки 51%»")


def is_threat(p):
    """Угроза есть, если доля мощности строго больше 0.5."""
    return p > 0.5


for p in [0.3, 0.51, 0.49, 0.6]:
    status = "УГРОЗА (атака 51%)" if is_threat(p) else "безопасно"
    print(f"Доля мощности {p:.0%}: {status}")


# Задание 8. Tangle (DAG): подтверждение двух транзакций
sep(8, "Tangle (DAG): подтверждение двух транзакций")

random.seed(42)
N_TX = 6
confirms = {}
for i in range(N_TX):
    if i < 2:
        confirms[i] = None
    else:
        confirms[i] = random.sample(range(i), 2)

for i in range(N_TX):
    if confirms[i] is None:
        print(f"Транзакция {i} подтверждает: – (начальная)")
    else:
        print(f"Транзакция {i} подтверждает: {confirms[i]}")


# Задание 9. Время подтверждения в IOTA
sep(9, "Время подтверждения в IOTA")


def confirm_time(k, n):
    """t = k / n — обратная пропорциональность числу транзакций."""
    return k / n


k = 1000
for n in [10, 100, 1000]:
    print(f"n={n:>5} транзакций -> время подтверждения = "
          f"{confirm_time(k, n):.2f} усл.ед.")


# Задание 10. Индексатор: транзакции и баланс адреса
sep(10, "Индексатор: транзакции и баланс адреса")

transactions = [
    ("Alice", "Bob", 100),
    ("Bob", "Alice", 30),
    ("Carol", "Alice", 50),
    ("Alice", "Dave", 20),
    ("Dave", "Bob", 10),
]


def index_address(txs, addr):
    """Возвращает (транзакции адреса, итоговый баланс)."""
    related = [t for t in txs if t[0] == addr or t[1] == addr]
    incoming = sum(t[2] for t in txs if t[1] == addr)
    outgoing = sum(t[2] for t in txs if t[0] == addr)
    return related, incoming - outgoing


addr = "Alice"
related, balance = index_address(transactions, addr)
print(f"Транзакции {addr} ({len(related)}):")
for frm, to, amount in related:
    print(f"  {frm} -> {to}: {amount}")
print(f"Итоговый баланс {addr}: {balance}")


# Задание 11. Статистика запросов по сабграфам
sep(11, "Статистика запросов по сабграфам")

log = [
    ("uniswap", 5000),
    ("aave", 3000),
    ("uniswap", 7500),
    ("compound", 4500),
    ("aave", 5500),
]


def subgraph_stats(log):
    d = {}
    for sg, cnt in log:
        d[sg] = d.get(sg, 0) + cnt
    busiest = max(d, key=d.get)
    return d, busiest, sum(d.values())


stats, busiest, total = subgraph_stats(log)
for sg, cnt in stats.items():
    print(f"{sg}: {cnt:,} запросов")
print(f"Самый нагруженный: {busiest}")
print(f"Всего запросов: {total:,}")


# Задание 12. Цифровая подпись (упрощённая модель)
sep(12, "Цифровая подпись (упрощённая модель)")


def sign(message, private_key):
    """Учебная подпись: SHA-256 от (сообщение + приватный ключ)."""
    return hashlib.sha256((message + private_key).encode()).hexdigest()


def verify(message, signature, private_key):
    """Проверка: пересчитываем подпись и сравниваем."""
    return sign(message, private_key) == signature


private_key = "secret_key_123"
message = "Перевести 1 BTC"

signature = sign(message, private_key)
print(f"Подпись: {signature[:24]}...")
print(f"Проверка верной подписи: {verify(message, signature, private_key)}")
print(f"Проверка при подмене сообщения: "
      f"{verify('Перевести 100 BTC', signature, private_key)}")


# Задание 13. Кубиты: число состояний
sep(13, "Кубиты: число состояний")


def quantum_states(n):
    """n кубитов хранят 2**n состояний одновременно."""
    return 2 ** n


for n in [1, 2, 8, 127]:
    print(f"{n:>4} кубит(ов): {quantum_states(n):,} состояний одновременно "
          f"| классика: {n} состояний")


# Задание 14. Алгоритм Шора: факторизация числа
sep(14, "Алгоритм Шора: факторизация числа")


def factorize(n):
    """Классическое разложение на простые множители."""
    factors = []
    d = 2
    while d * d <= n:
        while n % d == 0:
            factors.append(d)
            n //= d
        d += 1
    if n > 1:
        factors.append(n)
    return factors


for num in [15, 21, 91]:
    print(f"{num} = " + " x ".join(map(str, factorize(num))))


# Задание 15. Дорожная карта кубитов: экстраполяция роста
sep(15, "Дорожная карта кубитов: экстраполяция роста")

generations = [127, 433, 1121]

growth = [generations[i + 1] / generations[i] for i in range(len(generations) - 1)]
avg_growth = sum(growth) / len(growth)
forecast = round(generations[-1] * avg_growth)

print(f"Поколения кубитов: {generations}")
print(f"Коэффициенты роста: {[round(g, 2) for g in growth]}")
print(f"Средний темп роста: x{avg_growth:.2f}")
print(f"Прогноз следующего поколения: ~{forecast} кубитов")


print("\n" + "-" * 62)
print("ВСЕ ЗАДАНИЯ ВЫПОЛНЕНЫ")
print("-" * 62)
