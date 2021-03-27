# Suvastras Sample Code 
# We designed our own structre for coding in Laravel
# Controllers

<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use App\Http\Controllers\Base\BaseController;
use App\Http\Repositories\CategoryRepository;
use App\Http\Requests\CategoryRequest;
use Illuminate\Support\Str;

class CategoryController extends BaseController
{
	/**
	* @var  CategoryRepository $categoryRepository - Category Repository
	*/
	protected $categoryRepository;

	/**
	* Create a new controller instance.
	*
	* @return void
	*/
    public function __construct(CategoryRepository $categoryRepository)
    {
        $this->categoryRepository = $categoryRepository;
    }

	/**
	* Display a listing of the resource.
	*
	* @return \Illuminate\Http\Response
	*/
    public function index($status)
    {

    	$categories = $this->categoryRepository->allCategories($status);

        return view("category.list", [
        	"categories" => $categories
        ]);
    }

	/**
	* Show the form for creating a new resource.
	*
	* @return \Illuminate\Http\Response
	*/
    public function create()
    {
        return view("category.add", [ "status"	=> config('custom.status') ]);
    }

	/**
	* Store a newly created resource in storage.
	*
	* @param  \Illuminate\Http\Request  $request
	* @return \Illuminate\Http\Response
	*/
    public function store(CategoryRequest $request)
    {

        if ($this->categoryRepository->create(array_merge($data, $request->all()))) {
        	return redirect()->route('categories.index', ["status" => 1])
        					->with('success','Category created successfully');
        }

        return redirect()->back()->with('error','Category not updated');
    }
}
# Models

# Views

# Requests

Request is class which is use to handle form request for validation. Normally we write validation in controller itself but we wrote in separate class. 

<?php

/*
* CategoryRequest.php - Request file
*
* This file is part of the Category component.
*-----------------------------------------------------------------------------*/

namespace App\Http\Requests;

use Illuminate\Validation\Factory as ValidatorFactory;
use Illuminate\Foundation\Http\FormRequest;
use Illuminate\Http\Request;

class CategoryRequest extends FormRequest
{

	/**
	* Determine if the user is authorized to make this request.
	*
	* @return bool
	*/
    public function authorize()
    {
        return true;
    }
    
    /**
     * Get the validation rules that apply to the request.
     *
     * @return bool
     *-----------------------------------------------------------------------*/
    public function rules()
    {
        return [
			'name' 			=> 'required|min:6|max:255',
			'description' 	=> 'required|max:5000',
			'status' 		=> 'required',
        ];
    }
}

# Repositories

	Repository is a class which is used here to write database queries. Normally write in controller itself but to minimize the redundancy of queries, we wrote all queries separately in single file.
  
<?php
namespace App\Http\Repositories;
use App\Category;
class CategoryRepository
{
	/**
	* fetch category
	*
	* @param    string $uid
	* @return    eloquent collection object
	*/
	public function fetch($uid)
	{
		return Category::where("uid", "=", $uid)->first();
	}

	/**
	* update category
	*
	* @param    eloquent collection object
	* @param    array $params
	* @return   eloquent collection object
	*/
	public function update($category, $params)
	{
		return $category->update($params);
	}

	/**
	* update category
	*
	* @param    eloquent collection object
	* @param    array $params
	* @return   eloquent collection object
	*/
	public function create($params)
	{
		return Category::create($params);
	}
}

# Laravel Mix(Webpack)

# Database Migrations

# Database Migrations

