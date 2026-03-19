# Modularization – Opdeling in modules (2p)

Het Restaurant Orderingsysteem wordt opgedeeld in **10 modules**. Elke module bestaat uit een `.h`-bestand (interface) en een `.c`-bestand (implementatie).

## Overzicht modules

| Module | Bestanden | Verantwoordelijkheid |
|--------|-----------|----------------------|
| `main` | `main.c` | Ingangspunt van het programma |
| `auth` | `auth.h`, `auth.c` | Inloggen en registreren |
| `menu` | `menu.h`, `menu.c` | Menubeheer |
| `order` | `order.h`, `order.c` | Bestelbeheer |
| `payment` | `payment.h`, `payment.c` | Betalingsverwerking |
| `user` | `user.h`, `user.c` | Gebruikersprofielen |
| `kitchen` | `kitchen.h`, `kitchen.c` | Keuken/Bar interface |
| `admin` | `admin.h`, `admin.c` | Beheerderspaneel |
| `review` | `review.h`, `review.c` | Recensies |
| `utils` | `utils.h`, `utils.c` | Gedeelde hulpfuncties |

## Bestandsstructuur

```
restaurant_system/
├── main.c
├── auth/
│   ├── auth.h
│   └── auth.c
├── menu/
│   ├── menu.h
│   └── menu.c
├── order/
│   ├── order.h
│   └── order.c
├── payment/
│   ├── payment.h
│   └── payment.c
├── user/
│   ├── user.h
│   └── user.c
├── kitchen/
│   ├── kitchen.h
│   └── kitchen.c
├── admin/
│   ├── admin.h
│   └── admin.c
├── review/
│   ├── review.h
│   └── review.c
└── utils/
    ├── utils.h
    └── utils.c
```
