# SpaceX Dashboard - Task-by-Task Implementation
--------------------------------------------------
#### The report is structured in a progressive format, allowing tasks to be executed sequentially while tracking progress effectively
----------------------------------------------
## Initial Setup
``` python
# base_app.py
import pandas as pd
import dash
from dash import html
from dash import dcc
from dash.dependencies import Input, Output
import plotly.express as px

# Read the SpaceX data
spacex_df = pd.read_csv("spacex_launch_dash.csv")
max_payload = spacex_df['Payload Mass (kg)'].max()
min_payload = spacex_df['Payload Mass (kg)'].min()

# Create the Dash app
app = dash.Dash(__name__)

# Basic layout without functionality
app.layout = html.Div(children=[
    html.H1('SpaceX Launch Records Dashboard',
            style={'textAlign': 'center', 'color': '#503D36', 'font-size': 40}),
    html.Br(),
    html.Div(id='success-pie-chart'),
    html.Br(),
    html.P("Payload range (Kg):"),
    html.Div(id='success-payload-scatter-chart'),
])

# Run the app
if __name__ == '__main__':
    app.run()

```
![`Initial_setup_output`](https://github.com/user-attachments/assets/ca6974c0-2ccf-4d5d-b122-d1f647f3f84c)


## Task 1  Add a Launch Site Drop-down Input Component
``` python
# task1_dropdown.py
import pandas as pd
import dash
from dash import html
from dash import dcc
from dash.dependencies import Input, Output
import plotly.express as px

spacex_df = pd.read_csv("spacex_launch_dash.csv")
max_payload = spacex_df['Payload Mass (kg)'].max()
min_payload = spacex_df['Payload Mass (kg)'].min()

app = dash.Dash(__name__)

# TASK 1: Add dropdown with all launch sites
app.layout = html.Div(children=[
    html.H1('SpaceX Launch Records Dashboard',
            style={'textAlign': 'center', 'color': '#503D36', 'font-size': 40}),
    
    # Dropdown for launch sites
    dcc.Dropdown(
        id='site-dropdown',
        options=[
            {'label': 'All Sites', 'value': 'ALL'},
            {'label': 'CCAFS LC-40', 'value': 'CCAFS LC-40'},
            {'label': 'VAFB SLC-4E', 'value': 'VAFB SLC-4E'},
            {'label': 'KSC LC-39A', 'value': 'KSC LC-39A'},
            {'label': 'CCAFS SLC-40', 'value': 'CCAFS SLC-40'}
        ],
        value='ALL',
        placeholder="Select a Launch Site here",
        searchable=True
    ),
    
    html.Br(),
    html.Div(id='success-pie-chart'),
    html.Br(),
    html.P("Payload range (Kg):"),
    html.Div(id='success-payload-scatter-chart'),
])

# Run the app
if __name__ == '__main__':
    app.run()

```
![Task1_output](https://github.com/user-attachments/assets/db06f949-86e7-4f4c-a4eb-e10f0155e5f3)


## TASK 2: Add a callback function to render success-pie-chart based on selected site dropdown

``` python
# task2_piechart.py
import pandas as pd
import dash
from dash import html
from dash import dcc
from dash.dependencies import Input, Output
import plotly.express as px

spacex_df = pd.read_csv("spacex_launch_dash.csv")
max_payload = spacex_df['Payload Mass (kg)'].max()
min_payload = spacex_df['Payload Mass (kg)'].min()

app = dash.Dash(__name__)

app.layout = html.Div(children=[
    html.H1('SpaceX Launch Records Dashboard',
            style={'textAlign': 'center', 'color': '#503D36', 'font-size': 40}),
    
    dcc.Dropdown(
        id='site-dropdown',
        options=[
            {'label': 'All Sites', 'value': 'ALL'},
            {'label': 'CCAFS LC-40', 'value': 'CCAFS LC-40'},
            {'label': 'VAFB SLC-4E', 'value': 'VAFB SLC-4E'},
            {'label': 'KSC LC-39A', 'value': 'KSC LC-39A'},
            {'label': 'CCAFS SLC-40', 'value': 'CCAFS SLC-40'}
        ],
        value='ALL',
        placeholder="Select a Launch Site here",
        searchable=True
    ),
    
    html.Br(),
    
    # Pie chart container
    html.Div(dcc.Graph(id='success-pie-chart')),
    
    html.Br(),
    html.P("Payload range (Kg):"),
    html.Div(id='success-payload-scatter-chart'),
])

# TASK 2: Add callback for pie chart
@app.callback(
    Output(component_id='success-pie-chart', component_property='figure'),
    Input(component_id='site-dropdown', component_property='value')
)
def update_pie_chart(selected_site):
    if selected_site == 'ALL':
        fig = px.pie(spacex_df, 
                    values='class', 
                    names='Launch Site', 
                    title='Total Success Launches By Site')
    else:
        filtered_df = spacex_df[spacex_df['Launch Site'] == selected_site]
        fig = px.pie(filtered_df,
                    names='class',
                    title=f'Success vs Failure for {selected_site}')
    return fig

# Run the app
if __name__ == '__main__':
    app.run()

```
![Task_2_output](https://github.com/user-attachments/assets/8e03be79-751d-4f37-9e5f-4841247da2ed)

### TASK 3: Add a Range Slider to Select Payload

```python
# task3_slider.py
import pandas as pd
import dash
from dash import html
from dash import dcc
from dash.dependencies import Input, Output
import plotly.express as px

spacex_df = pd.read_csv("spacex_launch_dash.csv")
max_payload = spacex_df['Payload Mass (kg)'].max()
min_payload = spacex_df['Payload Mass (kg)'].min()

app = dash.Dash(__name__)

app.layout = html.Div(children=[
    html.H1('SpaceX Launch Records Dashboard',
            style={'textAlign': 'center', 'color': '#503D36', 'font-size': 40}),
    
    dcc.Dropdown(
        id='site-dropdown',
        options=[
            {'label': 'All Sites', 'value': 'ALL'},
            {'label': 'CCAFS LC-40', 'value': 'CCAFS LC-40'},
            {'label': 'VAFB SLC-4E', 'value': 'VAFB SLC-4E'},
            {'label': 'KSC LC-39A', 'value': 'KSC LC-39A'},
            {'label': 'CCAFS SLC-40', 'value': 'CCAFS SLC-40'}
        ],
        value='ALL',
        placeholder="Select a Launch Site here",
        searchable=True
    ),
    
    html.Br(),
    html.Div(dcc.Graph(id='success-pie-chart')),
    html.Br(),
    
    # TASK 3: Add payload range slider
    html.P("Payload range (Kg):"),
    dcc.RangeSlider(
        id='payload-slider',
        min=0,
        max=10000,
        step=1000,
        marks={
            0: '0 kg',
            2500: '2500',
            5000: '5000',
            7500: '7500',
            10000: '10000 kg'
        },
        value=[min_payload, max_payload]
    ),
    
    html.Div(dcc.Graph(id='success-payload-scatter-chart')),
])

@app.callback(
    Output(component_id='success-pie-chart', component_property='figure'),
    Input(component_id='site-dropdown', component_property='value')
)
def update_pie_chart(selected_site):
    if selected_site == 'ALL':
        fig = px.pie(spacex_df, 
                    values='class', 
                    names='Launch Site', 
                    title='Total Success Launches By Site')
    else:
        filtered_df = spacex_df[spacex_df['Launch Site'] == selected_site]
        fig = px.pie(filtered_df,
                    names='class',
                    title=f'Success vs Failure for {selected_site}')
    return fig

if __name__ == '__main__':
    app.run()
```

![`Task_3_output`](https://github.com/user-attachments/assets/6708cb2f-2075-40c8-970a-e4c5b1557f8c)

### Task 4: Add Scatter Plot Callback

```python
# task4_scatterplot.py
import pandas as pd
import dash
from dash import html
from dash import dcc
from dash.dependencies import Input, Output
import plotly.express as px

spacex_df = pd.read_csv("spacex_launch_dash.csv")
max_payload = spacex_df['Payload Mass (kg)'].max()
min_payload = spacex_df['Payload Mass (kg)'].min()

app = dash.Dash(__name__)

app.layout = html.Div(children=[
    html.H1('SpaceX Launch Records Dashboard',
            style={'textAlign': 'center', 'color': '#503D36', 'font-size': 40}),
    
    dcc.Dropdown(
        id='site-dropdown',
        options=[
            {'label': 'All Sites', 'value': 'ALL'},
            {'label': 'CCAFS LC-40', 'value': 'CCAFS LC-40'},
            {'label': 'VAFB SLC-4E', 'value': 'VAFB SLC-4E'},
            {'label': 'KSC LC-39A', 'value': 'KSC LC-39A'},
            {'label': 'CCAFS SLC-40', 'value': 'CCAFS SLC-40'}
        ],
        value='ALL',
        placeholder="Select a Launch Site here",
        searchable=True
    ),
    
    html.Br(),
    html.Div(dcc.Graph(id='success-pie-chart')),
    html.Br(),
    
    html.P("Payload range (Kg):"),
    dcc.RangeSlider(
        id='payload-slider',
        min=0,
        max=10000,
        step=1000,
        marks={
            0: '0 kg',
            2500: '2500',
            5000: '5000',
            7500: '7500',
            10000: '10000 kg'
        },
        value=[min_payload, max_payload]
    ),
    
    html.Div(dcc.Graph(id='success-payload-scatter-chart')),
])

# TASK 2: Pie chart callback
@app.callback(
    Output(component_id='success-pie-chart', component_property='figure'),
    Input(component_id='site-dropdown', component_property='value')
)
def update_pie_chart(selected_site):
    if selected_site == 'ALL':
        fig = px.pie(spacex_df, 
                    values='class', 
                    names='Launch Site', 
                    title='Total Success Launches By Site')
    else:
        filtered_df = spacex_df[spacex_df['Launch Site'] == selected_site]
        fig = px.pie(filtered_df,
                    names='class',
                    title=f'Success vs Failure for {selected_site}')
    return fig

# TASK 4: Scatter plot callback
@app.callback(
    Output(component_id='success-payload-scatter-chart', component_property='figure'),
    [Input(component_id='site-dropdown', component_property='value'),
     Input(component_id='payload-slider', component_property='value')]
)
def update_scatter_plot(selected_site, payload_range):
    low, high = payload_range
    filtered_df = spacex_df[(spacex_df['Payload Mass (kg)'] >= low) & 
                          (spacex_df['Payload Mass (kg)'] <= high)]
    
    if selected_site == 'ALL':
        fig = px.scatter(
            filtered_df,
            x='Payload Mass (kg)',
            y='class',
            color='Booster Version Category',
            title='Payload vs. Launch Outcome (All Sites)'
        )
    else:
        filtered_df = filtered_df[filtered_df['Launch Site'] == selected_site]
        fig = px.scatter(
            filtered_df,
            x='Payload Mass (kg)',
            y='class',
            color='Booster Version Category',
            title=f'Payload vs. Launch Outcome for {selected_site}'
        )
    return fig

if __name__ == '__main__':
    app.run()
```
![`Task_4_output`](https://github.com/user-attachments/assets/4a2f2d8f-ff13-4643-84ed-cff38830851d)


## The Final code after completion of all tasks

``` python
import pandas as pd
import dash
from dash import html
from dash import dcc
from dash.dependencies import Input, Output
import plotly.express as px

# Read the SpaceX data
spacex_df = pd.read_csv("spacex_launch_dash.csv")
max_payload = spacex_df['Payload Mass (kg)'].max()
min_payload = spacex_df['Payload Mass (kg)'].min()

# Create the Dash app
app = dash.Dash(__name__)

# Custom styling
styles = {
    'title': {
        'textAlign': 'center',
        'color': '#000000',
        'font-size': 40,
        'font-weight': 'bold',
        'margin-bottom': '30px'
    },
    'slider-title': {
        'font-size': '20px',
        'font-weight': 'bold',
        'color': '#000000',
        'margin-top': '20px',
        'margin-bottom': '10px'
    },
    'chart-title': {
        'font-size': '24px',
        'font-weight': 'bold',
        'color': '#000000',
        'text-align': 'center'
    },
    'slider-container': {
        'width': '80%',
        'margin': '0 auto',
        'padding': '20px 0'
    },
    'scatter-container': {
        'width': '90%',
        'margin': '0 auto',
        'height': '500px'
    }
}

app.layout = html.Div(children=[
    html.H1('SpaceX Launch Records Dashboard', style=styles['title']),
    
    # Dropdown for launch sites
    dcc.Dropdown(
        id='site-dropdown',
        options=[
            {'label': 'All Sites', 'value': 'ALL'},
            {'label': 'CCAFS LC-40', 'value': 'CCAFS LC-40'},
            {'label': 'VAFB SLC-4E', 'value': 'VAFB SLC-4E'},
            {'label': 'KSC LC-39A', 'value': 'KSC LC-39A'},
            {'label': 'CCAFS SLC-40', 'value': 'CCAFS SLC-40'}
        ],
        value='ALL',
        placeholder="Select a Launch Site here",
        searchable=True,
        style={'width': '50%', 'margin': '0 auto 30px'}
    ),
    
    # Pie chart container
    html.Div(dcc.Graph(id='success-pie-chart'), 
    
    # Payload slider
    html.Div([
        html.P("Payload range (Kg):", style=styles['slider-title']),
        dcc.RangeSlider(
            id='payload-slider',
            min=0,
            max=10000,
            step=1000,
            marks={
                0: {'label': '0 kg', 'style': {'font-size': '14px'}},
                2500: {'label': '2500', 'style': {'font-size': '14px'}},
                5000: {'label': '5000', 'style': {'font-size': '14px'}},
                7500: {'label': '7500', 'style': {'font-size': '14px'}},
                10000: {'label': '10000 kg', 'style': {'font-size': '14px'}}
            },
            value=[min_payload, max_payload],
            tooltip={"placement": "bottom", "always_visible": True}
        )
    ], style=styles['slider-container']),
    
    # Scatter plot container
    html.Div(dcc.Graph(id='success-payload-scatter-chart'), style=styles['scatter-container'])
])

# Pie chart callback
@app.callback(
    Output(component_id='success-pie-chart', component_property='figure'),
    Input(component_id='site-dropdown', component_property='value')
)
def update_pie_chart(selected_site):
    if selected_site == 'ALL':
        fig = px.pie(
            spacex_df, 
            values='class', 
            names='Launch Site', 
            title='Total Success Launches By Site'
        )
    else:
        filtered_df = spacex_df[spacex_df['Launch Site'] == selected_site]
        fig = px.pie(
            filtered_df,
            names='class',
            title=f'Success vs Failure for {selected_site}'
        )
    
    # Update pie chart styling
    fig.update_layout(
        title={
            'text': fig.layout.title.text,
            'font': {
                'size': 24,
                'color': 'black',
                'family': "Arial",
                'weight': 'bold'
            },
            'x': 0.5,
            'y': 0.95
        },
        legend={
            'font': {'size': 14},
            'orientation': 'v',
            'x': 1.05,
            'y': 0.5
        },
        margin=dict(l=50, r=150, t=80, b=50)  # Adjust margins for legend
    )
    return fig

# Scatter plot callback
@app.callback(
    Output(component_id='success-payload-scatter-chart', component_property='figure'),
    [Input(component_id='site-dropdown', component_property='value'),
     Input(component_id='payload-slider', component_property='value')]
)
def update_scatter_plot(selected_site, payload_range):
    low, high = payload_range
    filtered_df = spacex_df[
        (spacex_df['Payload Mass (kg)'] >= low) & 
        (spacex_df['Payload Mass (kg)'] <= high)
    ]
    
    if selected_site == 'ALL':
        title = 'Payload vs. Launch Outcome (All Sites)'
    else:
        filtered_df = filtered_df[filtered_df['Launch Site'] == selected_site]
        title = f'Payload vs. Launch Outcome for {selected_site}'
    
    fig = px.scatter(
        filtered_df,
        x='Payload Mass (kg)',
        y='class',
        color='Booster Version Category',
        title=title,
        width=800,  # Reduced width
        height=500  # Fixed height
    )
    
    # Update scatter plot styling
    fig.update_layout(
        title={
            'text': title,
            'font': {
                'size': 24,
                'color': 'black',
                'family': "Arial",
                'weight': 'bold'
            },
            'x': 0.5,
            'y': 0.95
        },
        xaxis={
            'title': 'Payload Mass (kg)',
            'title_font': {'size': 16},  # Slightly reduced
            'tickfont': {'size': 12}
        },
        yaxis={
            'title': 'Launch<br>Outcome',  # Break into two lines
            'title_font': {'size': 16},  # Slightly reduced
            'tickfont': {'size': 12},
            'title_standoff': 10  # Reduce space between axis and title
        },
        legend={
            'title': 'Booster Version',
            'font': {'size': 14},  # Slightly reduced
            'itemsizing': 'constant'
        },
        margin=dict(l=80, r=80, t=80, b=80),  # Tighter margins
        autosize=False
    )
    
    # Customize y-axis labels to fit better
    fig.update_yaxes(
        tickvals=[0, 1],
        ticktext=['Failure', 'Success'],
        tickangle=0
    )
    
    return fig


if __name__ == '__main__':
    app.run()
```
![Final_output](https://github.com/user-attachments/assets/3eca433f-fd06-4798-8718-714d6e111009)
