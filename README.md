# FPGA-notes

## Vocabulary

1) ROM: a block of memory whose contents are fixed when the chip is programmed. It is given an address and it returns the data stored there. Here, each address holds one market message.

2) .mif (Memory Initialization File): a text file that tells Quartus what values to put in each ROM address. Python generates it from the market data, and Quartus bakes it into the FPGA at compile time. It looks roughly like this:
      WIDTH=32; DEPTH=500;
      ADDRESS_RADIX=HEX; DATA_RADIX=HEX;
      CONTENT BEGIN
          0 : 04A1F00C;
          1 : 8C203005;
      END;

3) RAM: memory you can read and write at runtime. The book uses RAM because quantities change.
4) Dual-port RAM: a RAM with two independent access ports. The book updates through port A while the VGA dashboard reads through port B at the same time, so they never fight over access.
5) FSM (finite state machine): control logic that is always in one state (FETCH, WAIT, DECODE, ...) and moves to the next based on inputs. It is the standard way to sequence multi-step operations.
6) Datapath: the hardware doing the computing (adders, multipliers, registers) paired with the FSM.
7) Combinational vs. sequential: combinational logic outputs depend only on current inputs and have no clock. Sequential logic uses registers and a clock.
8) Handshake (valid/ready): the sender raises valid while the data is stable. The receiver raises ready or done when it has consumed it. This keeps modules in sync.
9) Exchange: the venue that matches buyers and sellers (e.g., NASDAQ). Ours is simulated by the matching engine.
10) Market data feed: the stream of messages from the exchange describing every change to the book.
11) Message types:
    Add: someone places a new order.
    Cancel: someone withdraws an order, identified only by its order ID.
    Trade: a buyer and a seller actually executed.
12) Bid: an order to buy.
13) Ask (offer): an order to sell.
14) Best bid: the highest price someone will pay.
15) Best ask: the lowest price someone will sell at.
16) Spread: best ask minus best bid.
17) Mid price: (best_bid + best_ask) / 2, the “fair” price.
18) Lot: one unit of the thing traded (you trade 1 lot per fill).

Blocks:
19) Feed Handler: Kinda like the front door. It reads raw 32-bit messages from the ROM, splits them in fields (type, side, price, ID, Quantity) and passes them on.
These messages convey the information like add, cancel, trade, ask, bid, quantity, price etc.

20) Feed handler FSM: its control sequence: FETCH (read the ROM address), WAIT (ROM takes one cycle to respond), DECODE (split the fields), SEND (give them to the book), WAIT_DONE (wait until the book finishes). Then it goes back to FETCH with the next address.
21) Feed handler FSM: its control sequence: FETCH (read the ROM address), WAIT (ROM takes one cycle to respond), DECODE (split the fields), SEND (give them to the book), WAIT_DONE (wait until the book finishes). Then it goes back to FETCH with the next address.
22) Latency counter: counts clock cycles from the message arriving (start pulse) to the order leaving (stop pulse). At 50 MHz, one cycle is 20 ns.
23) VGA dashboard: draws the book as bars, the quotes, and position and P&L on the monitor. The font/digit ROM stores the pixel patterns of the digits 0–9.
24) HEX display: the 7-segment digits on the board. They show best bid/ask and latency.
25) 
