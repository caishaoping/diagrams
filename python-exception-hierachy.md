```mermaid
flowchart LR

BaseException <-- Exception --- SystemExit --- KeyboardInterrupt
Exception <-- ArithmeticError --- LookupError --- TypeError --- ValueError
ArithmeticError <-- ZeroDivisionerror
LookupError <-- IndexError
LookupError <-- KeyError
