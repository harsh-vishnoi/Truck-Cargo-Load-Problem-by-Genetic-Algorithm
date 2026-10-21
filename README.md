# Truck cargo load by genetic algorithm

Chooses which loads to put on a truck or cargo ship when the hold has a weight limit and each load has a profit. The search is a genetic algorithm over a 0/1 knapsack: a gene is 1 when that load is taken and 0 when it is left behind.

`Cargo_load.cpp` is the program. `Cargo_data.csv` is the sample cargo list.

## Input

Each CSV row is `index,weight,benefit`. Weight is in tons and benefit is in crore rupees, matching the units printed at the end. The file must sit next to the binary, because the program opens `Cargo_data.csv` from the working directory.

On startup the program asks for the hold capacity, then builds a population of 100 random bit-strings (`population_size`) and runs 75 generations (`generations`).

## Steps in the code

1. `Create_Initial_Population` fills each individual with random 0/1 genes, one gene per cargo row.
2. `fitness` sums weight and benefit for the loads whose gene is 1.
3. `valid` / `make_valid` drop loads until the individual is under the capacity you typed.
4. `crossover` replaces the lowest-benefit individual with the first four genes of the best individual and the remaining genes of the second-best.
5. `mutation` flips one random gene on that same individual. `fitness_1` rescores it.
6. After 75 generations, `knapsack_ans` ranks the population by benefit per ton. The program then prints the bit-string, population index, weight, and benefit of the highest-benefit individual.

The program sleeps one second every 15 generations so the generation log is readable. It uses `unistd.h` (`sleep`), so build it on Linux or another POSIX system.

```bash
g++ Cargo_load.cpp -o cargo
./cargo
```

Enter the capacity in tons when prompted. A population that cannot be made valid for that capacity is rejected inside `make_valid`.

## Terms

| Term | In this program |
| --- | --- |
| Population | 100 candidate load lists |
| Chromosome | One candidate, a vector of 0 and 1 |
| Fitness | Total benefit of the loads that are both selected and under the weight limit |
| Crossover | Overwrites the weakest individual with a splice of the best and second-best |
| Mutation | Flips one gene of that weakest individual |
