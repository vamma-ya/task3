# task3

    def merge_intervals(intervals):
        if not intervals:
            return []
    
        intervals.sort()
        merged = [intervals[0]]

        for start, end in intervals[1:]:
            last_end = merged[-1][1]
            if start <= last_end:
                merged[-1][1] = max(last_end, end)
            else:
                merged.append([start, end])
        return merged

    def intersect_intervals(a, b):
        result = []
        i = j = 0

        while i < len(a) and j < len(b):
            start_a, end_a = a[i]
            start_b, end_b = b[j]
            start = max(start_a, start_b)
            end = min(end_a, end_b)

            if start < end:
                result.append([start, end])

            if end_a < end_b:
                i += 1
            else:
                j += 1

        return result

    def appearance(intervals: dict[str, list[int]]) -> int:
        lesson_start, lesson_end = intervals['lesson']
    
        def parse_and_trim(times):
            return [
                [max(times[i], lesson_start), min(times[i+1], lesson_end)]
                for i in range(0, len(times), 2)
                if max(times[i], lesson_start) < min(times[i+1], lesson_end)
            ]

        pupil_intervals = merge_intervals(parse_and_trim(intervals['pupil']))
        tutor_intervals = merge_intervals(parse_and_trim(intervals['tutor']))

    
        common_intervals = intersect_intervals(pupil_intervals, tutor_intervals)


    total_time = sum(end - start for start, end in common_intervals)
    return total_time
