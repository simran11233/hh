package main

import (
	"encoding/json"
	"fmt"
	"net/http"
	"strings"

	"github.com/google/uuid"
)

// Task represent a to-do task.
type Task struct {
	ID     string `json:"id"`
	Title  string `json:"title"`
	Status string `json:"status"`
}

var tasks = []Task{}

const Dport = ":8012"

func main() {
	http.HandleFunc("/task", taskHandler)
	http.HandleFunc("/tasks", tasksHandler)
	fmt.Printf("Server is starting on port: %v", Dport)
	http.ListenAndServe(Dport, nil)
}

func taskHandler(w http.ResponseWriter, r *http.Request) {
	// Extract the task ID from the URL path
	taskID := strings.TrimPrefix(r.URL.Path, "/task")
	switch r.Method {
	case "PUT":
		var updatedTask Task
		if err := json.NewDecoder(r.Body).Decode(&updatedTask); err != nil {
			http.Error(w, err.Error(), http.StatusBadRequest)
			return
		}
		found := false
		for i, task := range tasks {
			if task.ID == taskID {
				updatedTask.ID = taskID // Ensuring the ID remains unchanged
				tasks[i] = updatedTask
				found = true
				break
			}
		}
		if !found {
			http.Error(w, "Task not found", http.StatusNotFound)
		}
		json.NewEncoder(w).Encode(updatedTask)
	case "DELETE":
	default:
	}

}
func tasksHandler(w http.ResponseWriter, r *http.Request) {
	switch r.Method {
	case "GET":
		json.NewEncoder(w).Encode(tasks)
	case "POST":
		var task Task
		if err := json.NewDecoder(r.Body).Decode(&task); err != nil {
			http.Error(w, err.Error(), http.StatusBadRequest)
			return
		}
		// go get github.com/google/uuid
		task.ID = uuid.New().String()
		tasks = append(tasks, task)
		w.WriteHeader(http.StatusCreated)
		json.NewEncoder(w).Encode(tasks)
	default:
		w.WriteHeader(http.StatusMethodNotAllowed)
	}
}
