package coverage

import (
	"errors"
	"fmt"
	"os"
	"sort"
	"strings"
	"testing"
	"time"
)

// DO NOT EDIT THIS FUNCTION
func init() {
	content, err := os.ReadFile("students_test.go")
	if err != nil {
		panic(err)
	}
	err = os.WriteFile("autocode/students_test", content, 0644)
	if err != nil {
		panic(err)
	}
}

// WRITE YOUR CODE BELOW

func TestSwap(t *testing.T) {
	people := People{{firstName: "Laura"},{firstName: "Anel"}}
	peopleExpect := []string{"Anel","Laura"}

	people.Swap(0,1)
	if peopleExpect[0] != people[0].firstName && peopleExpect[1] != people[1].firstName {
		t.Errorf("func \"Swap()\" working not correctly")
	} 
}

func TestLen(t *testing.T) {
	people := People{{firstName: "Laura"},{firstName: "Anel"}}

	lP := people.Len()
	if lP != 2 {
		t.Errorf("func \"Len()\" working not correctly")
	} 
}

func TestLess(t *testing.T) {
	tPeople := map[string]map[string]interface{}{
		"birth=[1,0]" : {
			"value" : People{
				{birthDay: time.Date(1986, 12, 5, 0, 0, 0, 0, time.UTC)}, // oldest
				{birthDay: time.Date(1986, 12, 5, 0, 0, 1, 0, time.UTC)},
			},
			"expect" : true,
		},
		"birth=[0,1]" : {
			"value" : People{
				{birthDay: time.Date(1986, 12, 5, 0, 0, 1, 0, time.UTC)}, // younger
				{birthDay: time.Date(1986, 12, 5, 0, 0, 0, 0, time.UTC)},
			},
			"expect" : false,
		},
		"f_name[1,0]" : {
			"value" : People{
				{
					firstName: "abc#",
					birthDay: time.Date(1986, 12, 5, 0, 0, 0, 0, time.UTC),
				},
				{
					firstName: "abc",
					birthDay: time.Date(1986, 12, 5, 0, 0, 0, 0, time.UTC),
				},
			},
			"expect" : true,
		},
		"l_name[1,0]" : {
			"value" : People{
				{
					firstName: "abc",
					lastName: "abcd",
					birthDay: time.Date(1986, 12, 5, 0, 0, 0, 0, time.UTC),
				},
				{
					firstName: "abc",
					lastName: "abc",
					birthDay: time.Date(1986, 12, 5, 0, 0, 0, 0, time.UTC),
				},
			},
			"expect" : true,
		},
		"equal" : {
			"value" : People{
				{
					firstName: "abc",
					lastName: "abc",
					birthDay: time.Date(1986, 12, 5, 0, 0, 0, 0, time.UTC),
				},
				{
					firstName: "abc",
					lastName: "abc",
					birthDay: time.Date(1986, 12, 5, 0, 0, 0, 0, time.UTC),
				},
			},
			"expect" : false,
		},
	}

	for k, v := range tPeople {
		obj := v
		t.Run(k,func(t *testing.T) {
			t.Parallel()
			people := obj["value"].(People)
			expect := obj["expect"].(bool)
			got := people.Less(1, 0) // [i, i-1]
			if got != expect {
				t.Errorf("func \"Less()\" working not correctly: expect \"%t\" but got \"%t\"", expect, got)
			}
		})
	}
}

func TestPeopleLess(t *testing.T) {
	t.Parallel()
	people := People{{
		firstName: "Laura",
		lastName: "Galimzhanova",
		birthDay: time.Date(1986, 12, 5, 0, 0, 0, 0, time.UTC),
	},{
		firstName: "Anel",
		lastName: "Galimzhanova",
		birthDay: time.Date(2019, 12, 8, 0, 0, 0, 0, time.UTC),
	},{
		firstName: "Anel#",
		lastName: "Galimzhanova",
		birthDay: time.Date(2019, 12, 8, 0, 0, 0, 0, time.UTC),
	},{
		firstName: "Faiz",
		lastName: "Galimzhanov",
		birthDay: time.Date(1984, 8, 22, 0, 0, 0, 0, time.UTC),
	},{
		firstName: "Diana",
		lastName: "Galimzhanova",
		birthDay: time.Date(2015, 9, 21, 0, 0, 0, 0, time.UTC),
	},{
		firstName: "Adel",
		lastName: "Galimzhanova#",
		birthDay: time.Date(2017, 7, 20, 0, 0, 0, 0, time.UTC),
	},{
		firstName: "Adel",
		lastName: "Galimzhanova",
		birthDay: time.Date(2017, 7, 20, 0, 0, 0, 0,time.UTC),
	}}

	expect := []string{"Anel-Galimzhanova","Anel#-Galimzhanova","Adel-Galimzhanova",
	"Adel-Galimzhanova#","Diana-Galimzhanova","Laura-Galimzhanova","Faiz-Galimzhanov"}
	sort.Sort(people)
	if len(people) != len(expect) {
		t.Error("length expected ang got not same")
		return
	}
	for i, v := range people {
		got := fmt.Sprintf("%s-%s",v.firstName, v.lastName)
		if got != expect[i] {
			t.Errorf("sort: at index %d expected %s but got %s-%s",i, expect[i], v.firstName, v.lastName)
		}
	}
}

func TestNew(t *testing.T) {
	t.Parallel()
	tData := map[string]map[string]interface{}{
		"normal" : {
			"data" : "  0 1 2 3 4 5 6 7 8 9\n10 11 12 13 14 15 16 17 18 19  ",
			"exp_data" : Matrix{
				rows: 2,
				cols: 10,
				data: []int{0, 1, 2, 3, 4, 5, 6, 7, 8, 9,
					10, 11, 12, 13, 14, 15, 16, 17, 18, 19},
			},
			"exp_err" : nil,
		},
		"empty" : {
			"data" : " ",
			"exp_data" : nil,
			"exp_err" : errors.New("strconv.Atoi"),
		},
		"atoi_err" : {
			"data" : "0 1 2 3 4 5 6 7 8 9a\n 10 11 12 13 14 15 16 17 18 19",
			"exp_data" : nil,
			"exp_err" : errors.New("strconv.Atoi"),
		},
		"cols_err" : {
			"data" : "0 1 2 3 4 5 6 7 8 9\n 10 11 12 13 14 15 16 17 18",
			"exp_data" : nil,
			"exp_err" : errors.New("Rows need to be the same length"),
		},
	}

	for k, v := range tData {
		name, obj := k, v
		t.Run(name, func(t *testing.T) {
			t.Parallel()
			got, err := New(obj["data"].(string))
			if err != nil {
				exp_err := obj["exp_err"].(error)
				if ((name == "atoi_err" || name == "empty") && !strings.HasPrefix(err.Error(), exp_err.Error())) ||
				name == "cols_err" && err.Error() != exp_err.Error() {
					t.Fatalf("unexpected error: %s",err.Error())
				}
				return // for "ok error"
			}
			expire := obj["exp_data"].(Matrix)
			// catching error if testing data given with error
			if got.cols != expire.cols || got.rows != expire.rows || len(expire.data) != len(got.data) { 
				t.Fatalf("testing error: check given exp_data with given data, cols(%d-%d), rows(%d-%d), len(%d-%d),",
				expire.cols, got.cols,expire.rows,got.rows,len(expire.data),len(got.data))
			}
			// catching error if testing data given with error - end
			for i, d := range expire.data {
				if got.data[i] != d {
					t.Errorf("data not match: expected %d but got %d",d,got.data[i])
				}
			}
		})
	}
}

func TestRows(t *testing.T) {
	m := Matrix{
		rows: 2,
		cols: 10,
		data: []int{0, 1, 2, 3, 4, 5, 6, 7, 8, 9,
			10, 11, 12, 13, 14, 15, 16, 17, 18, 19},
	}
	expect := [][]int{
		{0, 1, 2, 3, 4, 5, 6, 7, 8, 9},
		{10, 11, 12, 13, 14, 15, 16, 17, 18, 19},
	}
	got := m.Rows()

	// catching error if testing data given with error
	lExpect := len(expect)
	if lExpect != m.rows {
		t.Fatalf("testing error: expected rows %d but got %d", lExpect, m.rows)
	}
	for i := 0; i < lExpect; i++ {
		if lCol := len(expect[i]); lCol != m.cols {
			t.Fatalf("testing error: expected cols %d but got %d", lCol, m.cols)
		} 
	}
	// catching error if testing data given with error - end
	for i := 0; i < lExpect; i++ {
		for j := 0; j < len(expect[i]);j++ {
			if got[i][j] != expect[i][j] {
				t.Fatalf("not match: expected value %d but got %d", expect[i][j], got[i][j])
			}
		}
	}
}

func TestCols(t *testing.T) {
	m := Matrix{
		rows: 2,
		cols: 10,
		data: []int{0, 1, 2, 3, 4, 5, 6, 7, 8, 9,
			10, 11, 12, 13, 14, 15, 16, 17, 18, 19},
	}
	expect := [][]int{{0, 10},{1, 11}, {2, 12}, {3, 13}, {4, 14}, {5, 15}, 
	{6, 16}, {7, 17}, {8, 18}, {9, 19}}
	got := m.Cols()

	// catching error if testing data given with error
	lExpect := len(expect)
	if lExpect != m.cols {
		t.Fatalf("testing error: expected rows %d but got %d", lExpect, m.cols)
	}
	for i := 0; i < lExpect; i++ {
		if lCol := len(expect[i]); lCol != m.rows {
			t.Fatalf("testing error: expected cols %d but got %d", lCol, m.rows)
		} 
	}
	// catching error if testing data given with error - end
	for i := 0; i < lExpect; i++ {
		for j := 0; j < len(expect[i]);j++ {
			if got[i][j] != expect[i][j] {
				t.Fatalf("not match: expected value %d but got %d", expect[i][j], got[i][j])
			}
		}
	}
}

func TestSet(t *testing.T) {
	t.Parallel()
	m := &Matrix{
		rows: 2,
		cols: 10,
		data: []int{0, 1, 2, 3, 4, 5, 6, 7, 8, 9,
			10, 11, 12, 13, 14, 15, 16, 17, 18, 19},
	}
	tData := map[string]map[string]int{
		"normal" : {
			"col" : 8,
			"row" : 0,
			"value" : 7,
			"answer" : 1,
		},
		"err_col" : {
			"col" : 10,
			"row" : 0,
			"value" : 7,
			"answer" : 0,
		},
		"err_row" : {
			"col" : 8,
			"row" : 2,
			"value" : 7,
			"answer" : 0,
		},
		"err_minus" : {
			"col" : -8,
			"row" : 0,
			"value" : 7,
			"answer" : 0,
		},
	}

	for k, v := range tData {
		name, obj := k, v
		t.Run(name, func(t *testing.T) {
			t.Parallel()
			ans := obj["answer"] 
			got := m.Set(obj["row"], obj["col"], obj["value"])
			if got && ans == 0 || !got && ans == 1 {
				t.Errorf("expected \"%t\" but got \"%t\"",!got, got)
			}
		})
	}
}