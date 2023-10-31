#include <stdio.h>
#include <stdlib.h>
#define MAX_PROCESSES 100

int main() {
  int n;
  printf("Enter the number of processes: ");
  scanf("%d", &n);

  int arrival_time[MAX_PROCESSES];
  int burst_time[MAX_PROCESSES];
  int completed_time[MAX_PROCESSES] = {0};
  int total_waiting_time = 0;

  for (int i = 0; i < n; i++) {
    printf("Enter arrival time for process %d: ", i + 1);
    scanf("%d", &arrival_time[i]);
    printf("Enter burst time for process %d: ", i + 1);
    scanf("%d", &burst_time[i]);
  }

  int current_time = 0;
  int completed = 0;

  double avg_waiting_time = 0.0;

  int priority_order[MAX_PROCESSES];  // To store the order of execution
  int order_index = 0;

  printf("\nRun Time\tPriority\tGantt Chart\tWaiting Time\tPriority Order\n");

  while (completed < n) {
    int idx = -1;
    double highest_priority = -1;

    for (int i = 0; i < n; i++) {
      if (arrival_time[i] <= current_time && completed_time[i] == 0) {
        double estimated_run_time = burst_time[i] + current_time - arrival_time[i];
        double priority = 1.0 + (current_time - arrival_time[i]) / estimated_run_time;
        if (priority > highest_priority) {
          highest_priority = priority;
          idx = i;
        }
      }
    }

    if (idx != -1) {
      completed_time[idx] = current_time + burst_time[idx];
      int waiting_time = completed_time[idx] - arrival_time[idx] - burst_time[idx];
      total_waiting_time += waiting_time;
      current_time += burst_time[idx];
      completed++;

      priority_order[order_index] = idx;  // Store the executed process
      order_index++;

      // Print the process name and priority order
      printf("%d ms\t\t%.2lf\t\t", burst_time[idx], highest_priority);
      for (int j = 0; j < burst_time[idx]; j++) {
        printf("P%d", idx + 1);
      }

      avg_waiting_time += waiting_time;

      // Print the waiting time and the priority order for the current process
      printf("\t\t%d ms\t\tP%d", waiting_time, idx + 1);

      printf("\n");
    } else {
      current_time++;
    }
  }

  avg_waiting_time /= n;

  printf("\nAverage Waiting Time = %lf ms\n", avg_waiting_time);

  // Print the priority order
  printf("\nPriority Order: ");
  for (int i = 0; i < n; i++) {
    printf("P%d", priority_order[i] + 1);
    if (i < n - 1) {
      printf(" -> ");
    }
  }

  return 0;
}
# os-code
