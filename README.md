# todo

```
package todo

import (
	"encoding/json"
	"fmt"
	"io/ioutil"
	"time"
)

type item struct {
	Task        string
	Done        bool
	CreatedAt   time.Time
	CompletedAt time.Time
}

type List []item

//create a new todo item and appends it to the list
func (l *List) add(task string) {
	t := item{
		Task:        task,
		Done:        false,
		CreatedAt:   time.Now(),
		CompletedAt: time.Time{},
	}

	*l = append(*l, t)
}

func (l *List) Compete(i int) error {
	ls := *l
	if i < 0 || i > len(ls) {
		return fmt.Errorf("Item %d doesn't exist.\n", i)
	}

	*l = append(ls[:i-1], ls[i:]...)

	return nil
}

func (l *List) Save(fileName string) error {
	js, err := json.Marshal(l)

	if err != nil {
		return err
	}

	return ioutil.WriteFile(fileName, js, 0644)
}

func (l *List) Get(filename string) error {
	file, err := ioutil.ReadFile(filename)

	if err != nil{
		if errors.Is(err, os.ErrNotExist){
			return nil
		}
		return err
	}

	if len(file) == 0{
		return nil
	}

	return json.Unmarshal(file, l)
}

```
