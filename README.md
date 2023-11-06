QUESTION NO 1
def merge_sort_and_count_inversions(arr):
    if len(arr) <= 1:
        return arr, 0

    mid = len(arr) // 2
    left_half = arr[:mid]
    right_half = arr[mid:]

    left_half, left_count = merge_sort_and_count_inversions(left_half)
    right_half, right_count = merge_sort_and_count_inversions(right_half)

    merged, merge_count = merge_and_count_inversions(left_half, right_half)

    total_count = left_count + right_count + merge_count

    return merged, total_count

def merge_and_count_inversions(left, right):
    merged = []
    count = 0
    i = j = 0

    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            merged.append(left[i])
            i += 1
        else:
            merged.append(right[j])
            j += 1
            count += len(left) - i

    merged.extend(left[i:])
    merged.extend(right[j:])

    return merged, count

# Example usage:
arr = [1, 3, 5, 2, 4, 6]
sorted_arr, inversions = merge_sort_and_count_inversions(arr)
print("Sorted Array:", sorted_arr)
print("Number of Inversions:", inversions)




QUESTION NO 2
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def merge_sort_linked_list(head):
    if not head or not head.next:
        return head

    # Find the middle of the linked list
    mid = find_middle(head)
    left_half = head
    right_half = mid.next
    mid.next = None

    # Recursively sort both halves
    left_sorted = merge_sort_linked_list(left_half)
    right_sorted = merge_sort_linked_list(right_half)

    # Merge the two sorted halves
    return merge(left_sorted, right_sorted)

def find_middle(head):
    slow = head
    fast = head

    while fast and fast.next and fast.next.next:
        slow = slow.next
        fast = fast.next.next

    return slow

def merge(left, right):
    dummy = ListNode(-1)
    current = dummy

    while left and right:
        if left.val < right.val:
            current.next = left
            left = left.next
        else:
            current.next = right
            right = right.next
        current = current.next

    if left:
        current.next = left
    if right:
        current.next = right

    return dummy.next

# Helper function to print the linked list
def print_linked_list(head):
    current = head
    while current:
        print(current.val, end=" -> ")
        current = current.next
    print("None")

# Example usage:
head = ListNode(4, ListNode(2, ListNode(1, ListNode(3, ListNode(5)))))
print("Original Linked List:")
print_linked_list(head)

sorted_head = merge_sort_linked_list(head)
print("\nSorted Linked List:")
print_linked_list(sorted_head)


QUESTION NO 3

def merge_sort_descending(arr):
    if len(arr) > 1:
        mid = len(arr) // 2
        left_half = arr[:mid]
        right_half = arr[mid:]

        merge_sort_descending(left_half)
        merge_sort_descending(right_half)

        i = j = k = 0

        while i < len(left_half) and j < len(right_half):
            if left_half[i] > right_half[j]:
                arr[k] = left_half[i]
                i += 1
            else:
                arr[k] = right_half[j]
                j += 1
            k += 1

        while i < len(left_half):
            arr[k] = left_half[i]
            i += 1
            k += 1

        while j < len(right_half):
            arr[k] = right_half[j]
            j += 1
            k += 1

# Example usage:
my_list = [12, 11, 13, 5, 6, 7]
merge_sort_descending(my_list)
print("Sorted list in descending order:", my_list)




QUESTION NO 4

def merge(arr, left, middle1, middle2, right):
    n1 = middle1 - left + 1
    n2 = middle2 - middle1
    n3 = right - middle2

    # Create temporary sublists to hold the elements of the three sublists
    left_sublist = arr[left:middle1 + 1]
    middle1_sublist = arr[middle1 + 1:middle2 + 1]
    middle2_sublist = arr[middle2 + 1:right + 1]

    i = j = k = 0
    p = left  # Index for the merged array

    while i < n1 and j < n2 and k < n3:
        if left_sublist[i] <= middle1_sublist[j] and left_sublist[i] <= middle2_sublist[k]:
            arr[p] = left_sublist[i]
            i += 1
        elif middle1_sublist[j] <= left_sublist[i] and middle1_sublist[j] <= middle2_sublist[k]:
            arr[p] = middle1_sublist[j]
            j += 1
        else:
            arr[p] = middle2_sublist[k]
            k += 1
        p += 1

    # Copy any remaining elements from the sublists
    while i < n1:
        arr[p] = left_sublist[i]
        i += 1
        p += 1
    while j < n2:
        arr[p] = middle1_sublist[j]
        j += 1
        p += 1
    while k < n3:
        arr[p] = middle2_sublist[k]
        k += 1
        p += 1

def three_way_merge_sort(arr, left, right):
    if left < right:
        # Calculate the middle points to divide the array into three parts
        middle1 = left + (right - left) // 3
        middle2 = left + 2 * (right - left) // 3

        # Recursively sort the three sublists
        three_way_merge_sort(arr, left, middle1)
        three_way_merge_sort(arr, middle1 + 1, middle2)
        three_way_merge_sort(arr, middle2 + 1, right)

        # Merge the three sublists
        merge(arr, left, middle1, middle2, right)

# Example usage:
arr = [12, 11, 13, 5, 6, 7]
three_way_merge_sort(arr, 0, len(arr) - 1)
print("Sorted array is:", arr)
