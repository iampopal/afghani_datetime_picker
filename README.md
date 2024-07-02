
### Simple:

```dart
class ExactDatePicker extends StatefulWidget {
  const ExactDatePicker({
    Key? key,
    required this.initalDate,
    required this.onSubmit,
  }) : super(key: key);
  final DateTime initalDate;
  final Function(DateTime date) onSubmit;

  static Future<DateTime?> pick(
      {required BuildContext context, required DateTime initalDate}) {
    return showDialog(
      context: context,
      builder: (context) {
        return CenterDialog(
          width: 400,
          child: ExactDatePicker(
            initalDate: initalDate,
            onSubmit: (res) {
              context.pop(res);
            },
          ),
        );
      },
    );
  }

  @override
  State<ExactDatePicker> createState() => _ExactDatePickerState();
}

class _ExactDatePickerState extends State<ExactDatePicker> {
  PDatePickerMode mode = PDatePickerMode.day;
  late DateTime date;

  @override
  void initState() {
    super.initState();
    date = widget.initalDate;
  }

  @override
  Widget build(BuildContext context) {
    final theme = context.theme.material.copyWith(
      colorScheme: context.theme.colorScheme.copyWith(
        primary: context.theme.primaryColor,
      ),
    );
    return Directionality(
      textDirection: TextDirection.rtl,
      child: Theme(
        data: theme,
        child: Column(
          children: [
            SizedBox(
              width: double.infinity,
              child: PDatePickerHeader(
                helpText: 'select hijri shamsi date'.localize(context),
                titleText: date.pickerTitle,
                titleStyle: context.theme.whiteTextTheme.headlineLarge
                    ?.copyWith(height: 0.8),
                orientation: Orientation.portrait,
                icon: null,
                iconTooltip: null,
                isShort: true,
                onIconPressed: () {},
              ),
            ),
            SizedBox(
              height: 340,
              child: ExactCalendarDatePicker(
                initialCalendarMode: mode,
                initialDate: Jalali.fromDateTime(widget.initalDate),
                firstDate: Jalali.fromDateTime(ServerUtils.firstDate),
                lastDate: Jalali.now(),
                onDateChanged: (Jalali? value) {
                  setState(() {
                    if (value != null) {
                      date = value.toDateTime();
                    }
                  });
                },
              ),
            ),
            AppButton(
              onTap: () {
                widget.onSubmit(date);
              },
              color: theme.colorScheme.primary,
              outlined: context.isDesktop,
              height: 42,
              margin: Spaces.tinyAll,
              width: double.infinity,
              text: 'Select',
            ),
          ],
        ),
      ),
    );
  }
}

```