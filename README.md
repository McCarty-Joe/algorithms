# algorithms
README for:
tsp.cpp 

DESCRIPTION:
For entry into the Traveling Salesman Problem (TSP) competition in CS325.


INPUT:
This program takes a command line input of a text file (*.txt) that contains a list of cities and their X & Y coordinates. The input format is as specified below:
* Each line defines a city and has three numbers separated by white space.
* The first number is the city id.
* The second number is the city's x coordinate.
* The third number is the city's y cooridnate.

CALCULATION
This program using a nearest neighbor algorithm with a 2-opt optimization to approximate the best tour for the TSP.

OUTPUT:
This program outputs a file named [input_filname.txt].tour that contains the tour length and a list of cities visited in order. The output format is as specified below:
* Each line contains one number.
* The first line is the total length of the TSP tour.
* Each consecutive line is the city visited.

Approach to Algorithm
Our approach to finding solutions to the Traveling Salesman Problem (TSP) considers balancing speed (due to time constraints for the competition) and accuracy (as we are graded relative to the best tour). Therefore, our ideal algorithm is one that can create a tour quickly (but not necessarily optimally), then optimize to decrease tour length. We decided to use a heuristic approximation algorithm [1], instead of an exact algorithm, as this would meet our goal of producing quick solutions. We selected the nearest neighbor algorithm [3], as it is a relatively “simple” algorithm to use as our base, and its speed vs. accuracy was comparable to other more complicated algorithms [2]. For optimization, we chose the 2-opt optimization [4] for similar reasons: it was relatively “simple” to code and could be optimized for better performance later if there was time.

Description of Algorithms
The nearest neighbor algorithm is a greedy algorithm [3]. It works by having the salesman start in a random city, then repeatedly choosing the next closest city (marking those already visited) until all cities have been visited. The salesman then makes the trip back to original city. It will quickly yield a short tour, but rarely will it find the most optimal. The algorithmic complexity is O(n^2). The 2-opt optimization is a simple local search algorithm used to optimize the results of an already valid TSP tour [4]. It works by utilizing a pairwise exchange. Two cities in the tour are swapped, and the new distance is checked to see if it is lower than the old tour. If it is lower, it becomes the new tour. All pairs are iterated through for each iteration of 2-opt. The 2-opt optimization is continued until there is no more improvement in the tour distance. Each iteration of 2-opt is O(n^2). Combined, the complexity is O(n^2) + O(n^2) = O(n^2). If we iterate through all possible starting cities, we will gain another n and the complexity becomes O(n^3).

Program Structure and Pseudo Code
Our program is structured to give us a quick solution from the nearest neighbor algorithm and then make it better using the 2-opt optimization. This is repeated for every possible starting city.

The main body of the program:
//repeat for all possible starting cities
for (i from 0 to number of cities){
   //call nearest neighbor algorithm starting from city i
   nearest(i, city_data, city_tour);
   //call 2-opt to optimize tour
   two_opt(city_data, city_tour);
}

The nearest neighbor algorithm:
void nearest(start_city, city_data, city_tour):
   //sets the starting city
   city_data[start_city] placed in city_tour
   city_data[start_city] is marked as visited
   current_city = start_city
   tour_distance = 0
   //iterate through all of the nearest neighbors
   for (i from 0 to number of cities - 1):
      min_distance = INT_MAX
      for (j from 0 to number of cities):
         if (city_data[j] has not been visited):
            temp_distance = distance (city_data[current_city] to city_data[j])
            if (temp_distance < min_distance):
               min_distance = temp_distance
               next_city = j      	
    	//add the shortest distance and make the next city the current city
    	tour_distance += min_distance
    	city_data[next_city] placed in city_tour
    	city_data[next_city] is marked as visited
    	current_city = next_city
   //add distance from last city to starting city
   tour_distance += distance(city_data[current_city], city_data[start_city])
   //writes the optimum tour to the output file
   if (tour_distance < best_distance):
      write city_tour to output_file
      best_distance = tour_distance

The 2-opt optimization:
void two_opt(city_data, city_tour):
   old_distance = INT_MAX
   tour_distance = distance of city_tour
   //perform iterations of 2-opt swap until the tour length does not decrease
   while (old_distance != tour_distance):
      old_distance = tour_distance
      for (i from 0 to number of cities - 1):
         for (k from i + 1 to number of cities):
            opt_change, tour_change = 0
            //calculates the change in distance before and after 2-opt swap
            opt_change = distance(reverse(city_order from i to k))
            tour_change = distance(city_order from i to k)
            opt_distance = tour_distance - tour_change + opt_change
            //compares new vs old distances
            if (opt_distance < tour_distance):
               //sets the new distance
               tour_distance = opt_distance
               //performs the 2-opt swap
               reverse(city_order from i to k)
      //check to see if distance has changed
    	if (old_distance != tour_distance):
         //writes the optimum tour to the output file
         if (tour_distance < best_distance):
            write city_tour to output_file
            best_distance = tour_distance

Improvements
If we had more time to complete this project, there are several improvements that we could possibly make to speed up the solution time and improve the accuracy:
	Pre-calculate or dynamically calculate the distances:  Currently, the distance between each city is calculated individually each time. If all of the distances were calculated upfront the program would run faster for larger city arrays, but would use more memory. The array could also be populated using dynamic programming.
	Use a different optimization method (such as 3-opt): Changing more than 2 vertices for each iteration has the potential to greatly increase the optimality of the tour found, but at the expense of slower operation.
	Optimize the programming code: Out code as it stands is written in C++ and uses vectors. Many of the vector functions could be optimized to minimize the number of assignments inside the for loops.

References
[1] “Travelling salesman problem”. Wikipedia. http://en.wikipedia.org/wiki/Travelling_salesman_problem
[2] “Comparison of TSP Algorithms”. Byung-In Kim, Jae-Ik Shim, Min Zhang. December 1998. http://bardzo.be/0sem/NAI/rozne/Comparison%20of%20TSP%20Algorithms/Comparison%20of%20TSP%20Algorithms.PDF
[3] “Nearest neighbour algorithm”. Wikipedia. http://en.wikipedia.org/wiki/Nearest_neighbour_algorithm
[4] “2-opt”. Wikipedia. http://en.wikipedia.org/wiki/2-opt
